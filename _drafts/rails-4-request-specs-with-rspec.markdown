---
layout: stickie
title: INtegration testing Rails 4 with Rspecs Request Specs
meta: rspec rails request spec http integration test
---

{{title}}

Rails ships with integration tests out the box. This means you can test your stack in a similar way to using Cucumber.

If you see something like this in your rails config:

    undefined method `hostname' on an instance of Rails::Application::Configuration.

These are the available assertions

    assert                  assert_in_epsilon       assert_no_tag           assert_not_kind_of      assert_nothing_thrown   assert_respond_to       assert_send
    assert_block            assert_include          assert_not_empty        assert_not_match        assert_operator         assert_response         assert_silent
    assert_dom_equal        assert_includes         assert_not_equal        assert_not_nil          assert_output           assert_routing          assert_tag
    assert_dom_not_equal    assert_instance_of      assert_not_in_delta     assert_not_operator     assert_predicate        assert_same             assert_template
    assert_empty            assert_kind_of          assert_not_in_epsilon   assert_not_predicate    assert_raise            assert_select
