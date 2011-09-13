---
layout: post
title: "Something Something Ruby Something"
date: 2011-09-13 16:56
comments: true
categories: 
---
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec ac euismod nunc. Etiam faucibus odio nec tellus tincidunt eu laoreet ipsum ultrices. Aenean at dui nunc, eget tincidunt erat. Sed at mi eu neque hendrerit aliquam faucibus pretium urna. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Pellentesque nisi elit, vestibulum ornare venenatis ac, commodo vitae lorem. Sed sit amet suscipit lacus. Praesent dapibus elit quis velit auctor vitae faucibus urna suscipit. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Ut a commodo felis. Suspendisse orci elit, luctus non commodo sit amet, feugiat sed risus. Nam et turpis quam, ut lobortis nunc. Sed ante ligula, porttitor vitae aliquet ac, sagittis sed risus. Ut pretium interdum risus a tempus. Vivamus aliquam ornare aliquam.

{% codeblock lang:ruby %}
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

Curabitur egestas justo eu nisl aliquam quis egestas metus bibendum. Morbi nibh massa, feugiat vitae vulputate at, imperdiet et arcu. Aliquam massa nunc, pharetra ut feugiat et, hendrerit sit amet ipsum. Sed nisl lectus, semper posuere auctor a, interdum ac ante. Sed ultricies nulla sed ipsum tempus a ullamcorper enim tempus. Aenean sed sem eget diam lacinia porttitor et vitae nunc. Sed blandit suscipit magna eu cursus. Mauris ac felis libero. Vivamus nec lorem mauris.

Maecenas ac porttitor lorem. In at erat cursus leo bibendum viverra vel et augue. Nulla sed turpis ut nunc euismod varius et in augue. Ut luctus justo sapien, id condimentum dolor. Nullam suscipit leo sit amet ante feugiat commodo ac eu lectus. Mauris fermentum, nisi fermentum aliquet suscipit, sem ante blandit dolor, sed eleifend dui risus ut turpis. Pellentesque fringilla felis ligula. Quisque at nunc ante. Integer dolor nisi, consectetur ut mattis in, egestas quis massa. Sed fermentum vestibulum sem quis hendrerit.

