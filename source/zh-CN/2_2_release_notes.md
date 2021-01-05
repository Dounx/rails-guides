**不要在 GITHUB 上阅读此文件，指南被发布在 https://guides.rubyonrails.org。**

Ruby on Rails 2.2 发行说明
===============================

Rails 2.2 提供了许多新功能并改进了许多原有功能。这份列表涵盖了主要的升级，但并不包括每个小的错误修复和更改。如果你想查看所有内容，请访问 GitHub 上 Rails 主存储库中的[提交列表](https://github.com/rails/rails/commits/2-2-stable)。

与 Rails 2.2 的一起发布的还有基于 [Rails Guides hackfest](http://hackfest.rubyonrails.org/guide) 的 [Ruby on Rails 指南](https://guides.rubyonrails.org/)。本站点将提供 Rails 主要功能的高质量文档。

--------------------------------------------------------------------------------

基础设施
--------------

Rails 2.2 对于基础设施而言是一个重要的发行版本，它让 Rails 保持活跃并与世界相连。

### 国际化

Rails 2.2 为国际化（i18n，对于那些讨厌打字的人）提供了一个简单的系统。

* 主要贡献者：Rails i18 小组
* 更多信息：
    * [官方 Rails i18 网站](http://rails-i18n.org)
    * [终于。Ruby on Rails 国际化了](https://web.archive.org/web/20140407075019/http://www.artweb-design.de/2008/7/18/finally-ruby-on-rails-gets-internationalized)
    * [本地化 Rails：演示应用](https://github.com/clemens/i18n_demo_app)

### 与 Ruby 1.9 和 JRuby 的兼容性

除了线程安全性之外，为了使 Rails 在 JRuby 和即将推出的 Ruby 1.9 上良好工作，开发者已经做了很多工作。随着 Ruby 1.9 的不断发展，在预览版本的 Ruby 上运行预览版本的 Rails 仍然是命中注定的命题，但是 Rails 准备在发布 Ruby 1.9 时过渡到 Ruby 1.9。

文档
-------------

Rails 的内部文档（代码注释）在许多地方得到了改进。此外，[Ruby on Rails 指南](https://guides.rubyonrails.org/)项目是有关主要 Rails 组件的权威信息来源。在其第一个正式版本中，指南页面包括：

* [Rails 入门](getting_started.html)
* [Rails 数据库迁移](active_record_migrations.html)
* [Active Record 关联](association_basics.html)
* [Active Record 查询接口](active_record_querying.html)
* [Rails 中的布局和渲染](layouts_and_rendering.html)
* [Action View 表单辅助方法](form_helpers.html)
* [Rails 路由全解](routing.html)
* [Action Controller 概览](action_controller_overview.html)
* [Rails 缓存](caching_with_rails.html)
* [Rails 应用测试指南](testing.html)
* [Ruby on Rails 安全指南](security.html)
* [调试 Rails 应用](debugging_rails_applications.html)
* [Rails 插件开发简介](plugins.html)

总而言之，这些指南为初学者和中级 Rails 开发人员提供了数以万计的指导。

如果要在本地生成这些指南，请在应用程序内部执行：

```bash
$ rake doc:guides
```

这会把指南放在 `Rails.root/doc/guides`，然后通过在你喜欢的浏览器中打开 `Rails.root/doc/guides/index.html` 来开始浏览。

* 主要的贡献来自于 [Xavier Noria](http://advogato.org/person/fxn/diary.html) 和 [Hongli Lai](http://izumi.plan99.net/blog/)。
* 更多信息：
    * [Rails Guides hackfest](http://hackfest.rubyonrails.org/guide)
    * [帮助改善 Git 分支上的 Rails 文档](https://weblog.rubyonrails.org/2008/5/2/help-improve-rails-documentation-on-git-branch)

与 HTTP 更好地集成：开箱即用的 ETag 支持
----------------------------------------------------------

在 HTTP 标头中支持 etag 和上次修改的时间戳意味着，如果 Rails 收到对最近未修改的资源的请求，则可以发回空响应。这使得你可以检查是否需要发送响应。

```ruby
class ArticlesController < ApplicationController
  def show_with_respond_to_block
    @article = Article.find(params[:id])

    # 如果请求发送的头部和传给 stale? 方法的选项不同，那么
    # 该请求确实过时且会触发 respond_to 块（传给 stale? 的选项也会在响应中设置）。
    #
    # 如果请求的头部匹配，那么该请求是新鲜的并且 respond_to 块不会被触发。
    # 这会检查 last-modified 和 etag 头部，然后得出结论：
    # 它只需要发送一个“304 Not Modified”而不是渲染对应的模版。
    if stale?(:last_modified => @article.published_at.utc, :etag => @article)
      respond_to do |wants|
        # 处理正常响应
      end
    end
  end

  def show_with_implied_render
    @article = Article.find(params[:id])

    # 设置响应的头部并与请求进行对比，如果请求是过时的
    # （比如没有匹配 etag 或 last-modified 中的任意一个），那么会渲染对应的模版。
    # 如果请求是新鲜的，那么默认渲染将会返回 “304 Not Modified” 而不是渲染模版。
    fresh_when(:last_modified => @article.published_at.utc, :etag => @article)
  end
end
```

线程安全
-------------

在 Rails 2.2 中实现了 Rails 的线程安全。根据你的 Web 服务器基础架构，可以使用更少的 Rails 副本（在内存中）来处理更多请求，从而提高服务器性能并提高多核利用率。

要在应用程序的生产模式下启用多线程调度，请在 `config/environments/production.rb` 中添加以下行：

```ruby
config.threadsafe!
```

* 更多信息：
    * [Rails 的线程安全](http://m.onkey.org/2008/10/23/thread-safety-for-your-rails)
    * [线程安全项目公告](https://weblog.rubyonrails.org/2008/8/16/josh-peek-officially-joins-the-rails-core)
    * [Q/A: 线程安全的 Rails 意味着什么](http://blog.headius.com/2008/08/qa-what-thread-safe-rails-means.html)

Active Record
-------------

这里会讨论两个重要的新功能：迁移事务化和数据库连接池。对于指定连接表的连接条件，还有一种新的（更简洁的）语法，以及许多较小的改进。

### 迁移事务化

从历史上看，多步 Rails 迁移一直是麻烦的根源。如果在迁移过程中出现问题，则错误之前的所有内容都会更改数据库，而错误之后的所有内容则不会。同样，迁移版本被存储为已执行，这意味着在解决问题后，不能通过 `rake db:migrate:redo` 简单地重新运行它。迁移事务化通过将迁移步骤包装在 DDL 事务中来改变这种情况，因此，如果其中任何一个失败，则整个迁移都将被撤消。在 Rails 2.2 中，开箱即用的 PostgreSQL 支持迁移事务化。该代码将来可以扩展到其他数据库类型，IBM 已经对其进行了扩展以支持 DB2 适配器。

* 主要贡献者：[Adam Wiggins](http://about.adamwiggins.com/)
* 更多信息：
    * [DDL 事务](http://adam.heroku.com/past/2008/9/3/ddl_transactions/)
    * [Rails 中 DB2 的一个重要里程碑](http://db2onrails.com/2008/11/08/a-major-milestone-for-db2-on-rails/)

### 连接池

通过连接池，Rails 可以在数据库连接池中分配数据库请求，数据库连接池会增长到最大大小（默认为 5，但是你可以在 `database.yml` 中添加 `pool` 选项来进行调整）。这有助于消除应用程序中的许多用户并发访问数据库的瓶颈。还有一个 `wait_timeout` 选项，默认为 5 秒后放弃。如果需要，可以通过 ActiveRecord::Base.connection_pool 来直接访问该池。

```yaml
development:
  adapter: mysql
  username: root
  database: sample_development
  pool: 10
  wait_timeout: 10
```

* 主要贡献者：[Nick Sieger](http://blog.nicksieger.com/)
* 更多信息：
    * [预览版本 Rails 的新增功能：连接池](http://archives.ryandaigle.com/articles/2008/9/7/what-s-new-in-edge-rails-connection-pools)

### 使用哈希来为连接表指定连接条件

现在你可以使用哈希来为连接表指定连接条件。这对于查询复杂的连接是一个巨大的帮助。

```ruby
class Photo < ActiveRecord::Base
  belongs_to :product
end

class Product < ActiveRecord::Base
  has_many :photos
end

# 获取所有的 products 以及免版权的 photos:
Product.all(:joins => :photos, :conditions => { :photos => { :copyright => false }})
```

* 更多信息：
    * [预览版本 Rails 的新功能：简单的连接表条件](http://archives.ryandaigle.com/articles/2008/7/7/what-s-new-in-edge-rails-easy-join-table-conditions)

### 新的动态查找器

Active Record 的动态查找器系列中增加了两组新方法。

#### `find_last_by_attribute`

`find_last_by_attribute` 方法等价于 `Model.last(:conditions => {:attribute => value})`

```ruby
# 获取最后一个在伦敦注册的 user
User.find_last_by_city('London')
```

* 主要贡献者：[Emilio Tagua](http://www.workingwithrails.com/person/9147-emilio-tagua)

#### `find_by_attribute!`

新的 bang! 版本的 `find_by_attribute!` 等价于 `Model.first(:conditions => {:attribute => value}) || raise ActiveRecord::RecordNotFound` 如果找不到匹配的记录，这个方法将会抛出异常而不是返回 `nil`。

```ruby
# 如果“Moby”还没有注册，则抛出 ActiveRecord::RecordNotFound
User.find_by_name!('Moby')
```

* 主要贡献者：[Josh Susser](http://blog.hasmanythrough.com)

### 不再支持调用关联对象的私有/受保护方法

Active Record 关联现在尊重关联对象方法的作用域。以前（假设 User has_one :account）`@user.account.private_method` 会调用关联 Account 对象的私有方法。这在 Rails 2.2 中会失败；如果你需要这个功能，你应当使用 `@user.account.send(:private_method)`（或者把私有/受保护方法改写成公共方法）。请主意如果你要覆盖 `method_missing`，则还需要覆盖 `respond_to` 来匹配行为，使得关联方法正常运行。

* 主要贡献者：Adam Milligan
* 更多信息：
    * [Rails 2.2 变化：关联对象的私有方法是私有的](http://afreshcup.com/2008/10/24/rails-22-change-private-methods-on-association-proxies-are-private/)

### 其它 Active Record 变化

* `rake db:migrate:redo` 现在接受一个可选的 VERSION 来指定重新运行特定迁移
* 设定 `config.active_record.timestamped_migrations = false` 来使得迁移使用数字前缀而不是 UTC 时间戳。
* 计数器缓存列 (用于声明的关联 `:counter_cache => true`) 不再需要初始化为 0。
* `ActiveRecord::Base.human_name` 用于模型名称的国际化人性化翻译

Action Controller
-----------------

在控制器方面，有一些更改可以帮助您整理路由。路由的内部也进行了一些更改，以降低复杂应用程序上的内存使用量。

### 浅层路由嵌套

浅层路由嵌套解决了使用深度嵌套资源时的困难。使用浅层嵌套，只需要提供足够的信息即可唯一地标识要使用的资源。

```ruby
map.resources :publishers, :shallow => true do |publisher|
  publisher.resources :magazines do |magazine|
    magazine.resources :photos
  end
end
```

这将使（其中包括）这些路由得以识别：

```
/publishers/1           ==> publisher_path(1)
/publishers/1/magazines ==> publisher_magazines_path(1)
/magazines/2            ==> magazine_path(2)
/magazines/2/photos     ==> magazines_photos_path(2)
/photos/3               ==> photo_path(3)
```

* 主要贡献者：[S. Brent Faulkner](http://www.unwwwired.net/)
* 更多信息：
    * [Rails 路由全解](routing.html#nested-resources)
    * [预览版本 Rails 的新增功能: 浅层路由](http://archives.ryandaigle.com/articles/2008/9/7/what-s-new-in-edge-rails-shallow-routes)

### 成员或集合路由的方法数组

现在，你可以提供方法数组给新的成员或集合路由。这样就解决了如果要处理一个或更多的动作，就要定义一个接受任何动词的路由的烦恼。对于 Rails 2.2，这是合法的路由声明：

```ruby
map.resources :photos, :collection => { :search => [:get, :post] }
```

* 主要贡献者：[Brennan Dunn](http://brennandunn.com/)

### 具有特定动作的资源

默认情况下，当使用 `map.resources` 创建路由时，Rails 会为七个默认动作（index，show，create，new，edit，update 以及 destroy）生成路由。但是这些路由中的每一个都会占用内存，并导致 Rails 生成额外的路由逻辑。现在则可以使用 `:only` 和 `:except` 选项来调整 Rails 为资源生成的路由。你可以提供一个动作，一系列动作或特殊的 `:all` 或 `:none` 选项。这些选项由嵌套资源继承。

```ruby
map.resources :photos, :only => [:index, :show]
map.resources :products, :except => :destroy
```

* 主要贡献者：[Tom Stuart](http://experthuman.com/)

### 其它 Action Controller 变化

* 现在你可以在路由请求异常时[显示自定义错误页面](http://m.onkey.org/2008/7/20/rescue-from-dispatching)。
* 现在默认禁用 HTTP Accept 标头。应当使用格式化的 URL（例如 `/customers/1.xml`）来指示所需的格式。如果需要 Accept 标头，则可以使用 `config.action_controller.use_accept_header = true` 将其重新打开。
* 基准测试数字现在以毫秒报告，而不是几分之一秒。
* Rails 现在支持仅 HTTP cookie（并将它们用于会话），这有助于减轻较新浏览器中的某些跨站脚本风险。
* `redirect_to` 现在完全支持 URI 方案（例如，您可以重定向到 svn`ssh: URI）。
* `render` 现在支持 `:js` 选项来使用正确的 mime 类型渲染普通的 JavaScript。
* 请求伪造保护已得到加强，仅适用于 HTML 格式的内容请求。
* 如果传递的参数为 nil，则多态 URLs 的行为会更为明智。例如，用值为 nil 的 date 调用 `polymorphic_path([@project, @date, @area])` 会得到`project_area_path`。

Action View
-----------

* `javascript_include_tag` 和 `stylesheet_link_tag` 新支持一个与 `:all` 一起使用的 `:recursive` 选项，这样就可以用一行代码来加载整个文件树。
* 内置的 Prototype JavaScript 库已升级到 1.6.0.3 版。
* `RJS#page.reload` 通过 JavaScript 重新加载浏览器的当前位置。
* 现在 `atom_feed` 辅助方法带有一个 `:instruct` 选项，可用于插入 XML 处理指令。

Action Mailer
-------------

Action Mailer 现在支持邮件布局。你可以通过提供正确命名的布局来使 HTML 电子邮件的外观与浏览器中的视图一样漂亮 - 例如，`CustomerMailer` 类希望使用l `ayouts/customer_mailer.html.erb`。

* 更多信息：
    * [预览版本 Rails 的新增功能：邮件布局](http://archives.ryandaigle.com/articles/2008/9/7/what-s-new-in-edge-rails-mailer-layouts)

现在，Action Mailer 通过自动打开 STARTTLS 为 GMail 的 SMTP 服务器提供内置支持。这需要安装 Ruby 1.8.7。

Active Support
--------------

现在 Active Support 为 Rails 应用程序提供了内置的记忆化方法，`each_with_object` 方法，为委托方法增加前缀以及各种新的工具方法。

### 记忆化

记忆化是一种模式：只初始化方法一次，然后将其值存放起来以备重复使用。你可能已经在自己的应用程序中使用了这种模式：

```ruby
def full_name
  @full_name ||= "#{first_name} #{last_name}"
end
```

记忆化可以让你以声明的方式实现此功能：

```ruby
extend ActiveSupport::Memoizable

def full_name
  "#{first_name} #{last_name}"
end
memoize :full_name
```

记忆化的其他功能包括 `unmemoize`，`unmemoize_all` 和 `memoize_all`，用于打开或关闭记忆化。

* 主要贡献者：[Josh Peek](http://joshpeek.com/)
* 更多信息：
    * [预览版本 Rails 的新功能：轻松记忆化](http://archives.ryandaigle.com/articles/2008/7/16/what-s-new-in-edge-rails-memoization)
    * [记忆什么？记忆化指南](http://www.railway.at/articles/2008/09/20/a-guide-to-memoization)

### each_with_object

通过使用 Ruby 1.9 向后兼容的方法，`each_with_object` 方法提供了 `inject` 的替代方法。它遍历一个集合，将当前元素和 memo 传递到块中。

```ruby
%w(foo bar).each_with_object({}) { |str, hsh| hsh[str] = str.upcase } # => {'foo' => 'FOO', 'bar' => 'BAR'}
```

主要贡献者：[Adam Keys](http://therealadam.com/)

### 为委托方法增加前缀

如果要将行为从一个类委托给另一个类，则现在可以指定一个前缀，该前缀将用于标识委托的方法。例如：

```ruby
class Vendor < ActiveRecord::Base
  has_one :account
  delegate :email, :password, :to => :account, :prefix => true
end
```

这将产生委托方法 `vendor＃account_email` 和 `vendor＃account_password`。你还可以指定一个自定义前缀：

```ruby
class Vendor < ActiveRecord::Base
  has_one :account
  delegate :email, :password, :to => :account, :prefix => :owner
end
```

这将产生委托方法 `vendor#owner_email` 和 `vendor#owner_password`。

主要贡献者：[Daniel Schierbeck](http://workingwithrails.com/person/5830-daniel-schierbeck)

### Active Support 的其它变化

* 对 `ActiveSupport::Multibyte` 的广泛更新，包括 Ruby 1.9 的兼容性修补程序。
* 新增 `ActiveSupport::Rescuable` 允许任何类 Mixin 来使用 `rescue_from` 语法。
* 在 `Date` 和 `Time` 类中新增 `past?`，`today?` 以及 `future?` 以便进行日期和时间的比较。
* `Array#second` 到 `Array#fifth` 作为 `Array#[1]` 到 `Array#[4]` 的别名。
* `Enumerable#many?` 封装了 `collection.size > 1`。
* `Inflector#parameterize` 产生其输入的 URL 就绪版本，以供 `to_param` 使用。
* `Time#advance` 可以识别小数天和星期，因此你可以执行 `1.7.weeks.ago`，`1.5.hours.since` 等。
* 包含的 TzInfo 库已升级到 0.3.12 版本。
* `ActiveSupport::StringInquirer` 为您提供了一种测试字符串是否相等的优雅方法：`ActiveSupport::StringInquirer.new("abc").abc? => true`。

Railties
--------

在 Railties（Rails 的核心代码）中，最大的变化是 config.gems 机制。

### config.gems

为了避免部署问题并使 Rails 应用程序更加独立，可以将 Rails 应用程序需要的所有 gem 的副本放置在 `/vendor/gems` 中。此功能最早出现在 Rails 2.1 中，但是在 Rails 2.2 中更加灵活和健壮，可以处理 gem 之间的复杂依赖关系。Rails中的 Gem 管理包括以下命令：

* 在 `config/environment.rb` 文件中的 `config.gem _gem_name_`
* `rake gems` 来列出所有已配置的 gem，以及它们（及其依赖项）是否已安装或冻结，以及是否为框架 gem（在执行 gem 依赖项代码之前由 Rails 加载的 gem；此类 gem 无法冻结）
* `rake gems:install` 安装缺少的 gem 到计算机
* `rake gems:unpack` 将所需 gem 的副本放入 `/vendor/gems`
* `rake gems:unpack:dependencies` 复制所需的 gem 及其依赖项的副本到 `/vendor/gems`
* `rake gems:build` 构建任何缺少的本地扩展
* `rake gems:refresh_specs` 使 Rails 2.1 创建的归档 gem 与 Rails 2.2 的存储方式保持一致

你可以通过在命令行上指定 `GEM=_gem_name_` 来解压缩或安装单个 gem。

* 主要贡献者：[Matt Jones](https://github.com/al2o3cr)
* 更多信息：
    * [预览版本 Rails 的新增功能：Gem 依赖性](http://archives.ryandaigle.com/articles/2008/4/1/what-s-new-in-edge-rails-gem-dependencies)
    * [Rails 2.1.2 和 2.2RC1: 更新你的 RubyGems](https://afreshcup.com/home/2008/10/25/rails-212-and-22rc1-update-your-rubygems)
    * [在 Lighthouse 的详细讨论](http://rails.lighthouseapp.com/projects/8994-ruby-on-rails/tickets/1128)

### Railties 的其它变化

* 如果你是 [Thin](http://code.macournoyer.com/thin/) Web 服务器的粉丝，你会很乐意知道 `script/server` 现在直接支持 Thin。
* `script/plugin install &lt;plugin&gt; -r &lt;revision&gt;` 现在可以与基于 git 以及基于 svn 的插件一起使用。
* `script/console` 现在支持一个 `--debugger` 选项。
* Rails 源代码中包含有关设置持续集成服务器以构建 Rails 本身的说明。
* `rake notes:custom ANNOTATION=MYFLAG` 让你列出自定义注释。
* 将 `Rails.env` 包装在 `StringInquirer` 中，以便你可以使用 `Rails.env.development?`。
* 为了消除过时警告并正确处理 gem 依赖项，Rails 现在需要 rubygems 1.3.1 或更高版本。

已弃用
----------

一些此版本已弃用的旧代码：

* `Rails::SecretKeyGenerator` 已经被 `ActiveSupport::SecureRandom` 替代。
* `render_component` 已弃用。如果需要此功能，可以使用 [render_components 插件](https://github.com/rails/render_component/tree/master)。
* 已弃用渲染局部视图时的隐式 local 赋值。

    ```ruby
    def partial_with_implicit_local_assignment
      @customer = Customer.new("Marcel")
      render :partial => "customer"
    end
    ```

    以前，上述代码在局部视图 'customer' 内部提供了一个名为 `customer` 的局部变量。现在应该通过 :locals 哈希显式传递所有变量。

* `country_select` 已被移除。有关更多信息以及插件更换，请参见[弃用页面](http://www.rubyonrails.org/deprecation/list-of-countries)。
* `ActiveRecord::Base.allow_concurrency` 不再有任何效果。
* `ActiveRecord::Errors.default_error_messages` 已弃用，推荐使用 `I18n.translate('activerecord.errors.messages')`。
* 不建议使用 `%s` 和 `%d` 插值语法进行国际化。
* `String#chars` 已弃用，推荐使用 `String#mb_chars`。
* 小数月或小数年的持续时间已弃用。请改用 Ruby 的核心 `Date` 和 `Time` 类算法。
* `Request#relative_url_root` 已弃用，推荐使用 `ActionController::Base.relative_url_root`。

工作人员
-------

由 [Mike Gunderloy](http://afreshcup.com) 编译的发行说明
