---
layout: stickie
title: Playing with arel
meta: Arel, ruby
---
users table stuff

    def arex query
      ActiveRecord::Base.connection.execute query
    end

    users = Arel::Table.new(:sf_guard_user)
    basic_query = users.project(Arel.sql(sql('*'))) # select all fields
    basic_query = users.project(users[:id]) # select one field

specify cartesian subset
    specific_query = users.project([users[:id], users[:username]])

... with conditions
    specific_query = users.project([users[:id], users[:username]]).where(users[:id].eq(1))

#### ... with a join, can get tricky
    join_query = users.project([users[:id], users[:username]]).join(:sf_guard_user_profile).on(users[:id].eq('sf_guard_user_profile.user_id'))
Returns ...
"SELECT \"sf_guard_user\".\"id\", \"sf_guard_user\".\"username\" FROM \"sf_guard_user\" INNER JOIN 'sf_guard_user_profile' ON \"sf_guard_user\".\"id\" = 0"

using additional Arel tables seems to clear this up

    profiles = Arel::Table.new(:sf_guard_user_profile)

    join_query = users.project([users[:id], users[:username]]).join(profiles).on(users[:id].eq(profiles['user_id']))

and you can relax it a bit too

    join_query = users.project('*').join(profiles).on(users[:id].eq(profiles['user_id']))

#### get a relation's columns

    users.columns.map(&:name)

UPDATNG TABLES

    crudder = Arel::SelectManager.new users.engine

    crudder.compile_update([[users[:username], "steveo@lyti.cs"]]).where(users[:id].eq(1)).to_sql
    #=> "UPDATE \"sf_guard_user\" SET \"username\" = 'steveo@lyti.cs' WHERE \"sf_guard_user\".\"id\" = 1"

    profiles = Arel::Table.new(:sf_guard_user_profile)
    user_groups = Arel::Table.new(:sf_guard_user_group)

## Get lender profiles
    profiles.project(Arel.star).join(user_groups).on(user_groups[:user_id].eq(profiles[:user_id])).where(user_groups[:group_id].eq(3))

    manager = Arel::SelectManager.new Table.engine

### Soo ... stored procedures :D

#### Could you express the following as Arel... ?
    ActiveRecord::Base.connection.execute "select id, (select * from financial.balance(u.id, true)) as bal from sf_guard_user as u where u.id = 109"
    ActiveRecord::Base.connection.execute "select id, (select * from financial.account_ref(u.id)) as bal from sf_guard_user as u where u.id = 109"

And the answer, as it turns out, is yes, somewhat ...
    users = Arel::Table.new(:sf_guard_user)
    users.table_alias = 'u'

    manager = Arel::SelectManager.new users.engine
              manager.project Arel.star
              manager.from Arel.sql("financial.account_ref(#{users.table_alias}.id)")
              # as = manager.as(Arel.sql('bal'))

IN sql you have column aliases (AS) and table (or *relation*) aliases (table_alias)
    users.project([users[:id], manager.as('bal')])#.from('u')#.as(Arel.sql('u'))
    arex users.project([users[:id], manager.as('bal')]).where(users[:id].eq(109)).to_sql

 sadly this is considerably more verbose than just using connection.execute with esql

    accounts = Arel::Table.new("financial.account")

Some failed experiements ....
the answer turned out to be: users.table_alias = 'u', see above

          # try to create an alias of u for sf_guard_user
          # users_manager = Arel::SelectManager.new users.engine
          # users_manager.from(users)#.as('u')
          # users_as = users_manager.as(Arel.sql('u'))
          # users_manager.project([users[:id], manager.as('bal')]).as('u').where(users[:id].eq(109)).to_sql

          # user_manager = Arel::SelectManager.new Table.engine
          # manager.project Arel.sql('id', as])
          # manager.from as
          # manager.to_sql.must_be_like "SELECT name FROM (SELECT * FROM zomg) foo"

The top level Arel classes

[58] pry(main)> accref = Arel::A
Arel::AliasPredication  Arel::Attribute         Arel::Attributes
[58] pry(main)> accref = Arel::C
Arel::Compatibility  Arel::Crud
[58] pry(main)> accref = Arel::DeleteManager
Arel::DeleteManager
[58] pry(main)> accref = Arel::Expression
Arel::Expression   Arel::Expressions
[58] pry(main)> accref = Arel::In
Arel::InnerJoin      Arel::InsertManager
[58] pry(main)> accref = Arel::Math
Arel::Math
[58] pry(main)> accref = Arel::Node
Arel::Node   Arel::Nodes
[58] pry(main)> accref = Arel::O
Arel::OrderPredications  Arel::OuterJoin
[58] pry(main)> accref = Arel::Predications
Arel::Predications
[58] pry(main)> accref = Arel::Relation
Arel::Relation
[58] pry(main)> accref = Arel::S
Arel::SelectManager  Arel::Sql            Arel::SqlLiteral
[58] pry(main)> accref = Arel::T
Arel::Table        Arel::TreeManager
[58] pry(main)> accref = Arel::UpdateManager
Arel::UpdateManager
[58] pry(main)> accref = Arel::V
Arel::VERSION   Arel::Visitors
[58] pry(main)> accref = Arel::Z


## Using arel for data analysis

The nice thing about active record is that it works for mvc really well
But if you want to do data analysis, such as working out the function of some data, it's not so good

So if we want to analyse bidding data:

    bids = Arel::Table.new(Bid.table_name)
    res = arex bids.project(bids[:id]).where(bids[:id].eq(1)).to_sql

How many bids placed by day of the week
    res = arex bids.project(bids[:id].count).group("extract(dow from #{bids.name}.created_at)").to_sql

... or day of the year
    res = arex bids.project(bids[:id].count).group("extract(doy from #{bids.name}.created_at)").to_sql

add in the date as a field, and get the number per month since launch
    res = arex bids.project(bids[:id].count, "date_trunc('month', #{bids.name}.created_at) as gdate").group("gdate").to_sql

get the number per day ...
    res = arex bids.project(bids[:id].count, "date_trunc('day', #{bids.name}.created_at) as group_date").group("group_date").to_sql

add in the status of the bid ...
    res = arex bids.project(bids[:id].count, "date_trunc('day', #{bids.name}.created_at) as group_date", bids[:status]).group("group_date", bids[:status]).to_sql


