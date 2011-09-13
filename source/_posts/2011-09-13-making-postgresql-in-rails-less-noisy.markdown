---
layout: post
title: "Making PostgreSQL in Rails Less Noisy"
date: 2011-09-13 17:43
comments: true
categories: postgresql rails ruby
---
There are often a lot of benefits to replacing MySQL with PostgreSQL in a Rails application, but there is one significant downside: Postgres has never really been a first class citizen in the Rails database world, and the integration isn't quite as bulletproof as MySQL's. In porting our applications over to Postgres the past few months, we've run into some minor annoyances. The worst of these was the `pg` gem's curious handling (or lack thereof) of Postgres's very verbose notice messages. These were severely polluting our logs and, more importantly, our spec output. There are a couple gems out there that purport to fix it, none of which we had any luck with. I ended up diving into the driver's source and came up with this:

{% codeblock lang:ruby config/initializers/silence_postgres_notices.rb %}
module ActiveRecord::ConnectionAdapters
  class PostgreSQLAdapter
    alias_method :__connect, :connect
    def connect
      __connect
      @connection.set_notice_receiver { |a| nil }
    end
  end
end
{% endcodeblock %}

Stick this anywhere in `config/initializers`, and your postgres experience should get a whole lot quieter.
