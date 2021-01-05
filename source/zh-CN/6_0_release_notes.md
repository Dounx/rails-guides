**不要在 GITHUB 上阅读此文件，指南被发布在 https://guides.rubyonrails.org。**

Ruby on Rails 6.0 发行说明
===============================

Rails 6.0 中的亮点：

* Action Mailbox
* Action Text
* Parallel Testing
* Action Cable Testing

这些发行说明仅涵盖主要变化。
要了解各种错误修复和变化，请参阅更改日志或查看 GitHub 上的[提交列表](https://github.com/rails/rails/commits/6-0-stable)。

--------------------------------------------------------------------------------

升级到 Rails 6.0
----------------------

如果要升级现有的应用程序，那么应该在升级之前拥有良好的测试覆盖。
以防万一，应该首先升级到 Rails 5.2，并确保应用程序仍按预期运行，然后再尝试更新到 Rails 6.0。
[升级 Ruby on Rails](upgrading_ruby_on_rails.html#upgrading-from-rails-5-2-to-rails-6-0) 指南中提供了升级时需要注意的事项列表。

主要功能
--------------

### Action Mailbox

[Pull Request](https://github.com/rails/rails/pull/34786)

[Action Mailbox](https://github.com/rails/rails/tree/6-0-stable/actionmailbox) 允许将收到的电子邮件路由到类似控制器的邮箱。你可以在 [Action Mailbox 基础](action_mailbox_basics.html) 指南中阅读有关 Action Mailbox 的更多信息。

### Action Text

[Pull Request](https://github.com/rails/rails/pull/34873)

[Action Text](https://github.com/rails/rails/tree/6-0-stable/actiontext) 将丰富的文本内容和编辑功能带入 Rails。这包括 [Trix 编辑器](https://trix-editor.org)，它可以处理格式设置、链接、引用、列表以及嵌入图像和图库。
Trix 编辑器生成的富文本内容将以其自己的格式保存在任何现有的 Active Record 模型相关联的 RichText 模型。
任何嵌入的图像（或其他附件）都将使用 Active Storage 自动存储并与对应的 RichText 模型相关联。

你可以在 [Action Text 概述](action_text_overview.html)指南中阅读有关 Action Text 的更多信息。

### 并行测试

[Pull Request](https://github.com/rails/rails/pull/31900)

[并行测试](testing.html#parallel-testing) 使你可以并行化测试套件。虽然 fork 进程是默认方法，但也支持线程。并行运行测试可以减少整个测试套件的运行时间。

### Action Cable 测试

[Pull Request](https://github.com/rails/rails/pull/33659)

[Action Cable 测试工具](testing.html#testing-action-cable) 允许你在任何级别上测试 Action Cable 的功能：connections，channels，broadcasts。

Railties
--------

有关变化详情，请参考[Changelog][railties]。

### 删除

*   删除插件模版中已弃用的 `after_bundle` 辅助方法。
    ([Commit](https://github.com/rails/rails/commit/4d51efe24e461a2a3ed562787308484cd48370c7))

*   删除已弃用的支持：`config.ru` 使用 `Rails::Application` 子类
    作为 `run` 的参数。
    ([Commit](https://github.com/rails/rails/commit/553b86fc751c751db504bcbe2d033eb2bb5b6a0b))

*   删除 rails 命令中已弃用的 `environment` 参数。
    ([Commit](https://github.com/rails/rails/commit/e20589c9be09c7272d73492d4b0f7b24e5595571))

*   删除生成器和模版中已弃用的 `capify!` 方法。
    ([Commit](https://github.com/rails/rails/commit/9d39f81d512e0d16a27e2e864ea2dd0e8dc41b17))

*   删除已弃用的 `config.secret_token`。
    ([Commit](https://github.com/rails/rails/commit/46ac5fe69a20d4539a15929fe48293e1809a26b0))

### 弃用

*   弃用将传递的 Rack 服务器名称作为 `rails server` 的常规参数使用。
    ([Pull Request](https://github.com/rails/rails/pull/32058))

*   弃用使用 `HOST` 环境变量指定服务器 IP。
    ([Pull Request](https://github.com/rails/rails/pull/32540))

*   弃用非符号键获取 `config_for` 返回的哈希值。
    ([Pull Request](https://github.com/rails/rails/pull/35198))

### 重要变化

*   新增 `rails server` 命令中的显式选项 `--using` 或 `-u` 来指定服务器。
    ([Pull Request](https://github.com/rails/rails/pull/32058))

*   新增以扩展格式查看 `rails routes` 输出的功能。
    ([Pull Request](https://github.com/rails/rails/pull/32130))

*   用内置的 Active Job 运行“种子”数据库任务。
    ([Pull Request](https://github.com/rails/rails/pull/34953))

*   新增一个命令 `rails db:system:change` 来更改应用程序的数据库种类。
    ([Pull Request](https://github.com/rails/rails/pull/34832))

*   新增 `rails test:channels` 命令来只运行 Action Cable channels 的测试。
    ([Pull Request](https://github.com/rails/rails/pull/34947))

*   引入防范 DNS 重新绑定攻击的方法。
    ([Pull Request](https://github.com/rails/rails/pull/33145))

*   新增在运行生成器命令失败时终止的功能。
    ([Pull Request](https://github.com/rails/rails/pull/34420))

*   使 Webpacker 成为 Rails 6 的默认 JavaScript 编译器。
    ([Pull Request](https://github.com/rails/rails/pull/33079))

*   为 `rails db:migrate:status` 命令添加多个数据库支持。
    ([Pull Request](https://github.com/rails/rails/pull/34137))

*   新增生成器中使用多个数据库的不同迁移路径的功能。
    ([Pull Request](https://github.com/rails/rails/pull/34021))

*   新增对多环境凭据的支持。
    ([Pull Request](https://github.com/rails/rails/pull/33521))

*   将 `null_store` 作为测试环境中的默认 cache store。
    ([Pull Request](https://github.com/rails/rails/pull/33773))

Action Cable
------------

有关变化详情，请参考[Changelog][action-cable]。

### 删除

*   用 `ActionCable.logger.enabled` 替代 `ActionCable.startDebugging()` 和 `ActionCable.stopDebugging()`。
    ([Pull Request](https://github.com/rails/rails/pull/34370))

### 弃用

*   Rails 6.0 中 Action Cable 没有弃用。

### 重要变化

*   在 `cable.yml` 中添加对 PostgreSQL 订阅适配器的 `channel_prefix` 选项的支持。
    ([Pull Request](https://github.com/rails/rails/pull/35276))

*   允许传自定义配置到 `ActionCable::Server::Base`。
    ([Pull Request](https://github.com/rails/rails/pull/34714))

*   新增 `:action_cable_connection` 和 `:action_cable_channel` 加载钩子方法。
    ([Pull Request](https://github.com/rails/rails/pull/35094))

*   新增 `Channel::Base#broadcast_to` 和 `Channel::Base.broadcasting_for`。
    ([Pull Request](https://github.com/rails/rails/pull/35021))

*   在 `ActionCable::Connection` 中调用 `reject_unauthorized_connection` 会关闭一个连接。
    ([Pull Request](https://github.com/rails/rails/pull/34194))

*   将 Action Cable JavaScript 程序包从 CoffeeScript 转换为 ES2015，并在 npm 中发布源代码。
    ([Pull Request](https://github.com/rails/rails/pull/34370))

*   将 WebSocket 适配器和日志适配器的配置从 `ActionCable` 的属性移动到`ActionCable.adapters`。
    ([Pull Request](https://github.com/rails/rails/pull/34370))

*   在 Redis 适配器中添加一个 `id` 选项，以区分 Action Cable 的 Redis 连接。
    ([Pull Request](https://github.com/rails/rails/pull/33798))

Action Pack
-----------

有关变化详情，请参考[Changelog][action-pack]。

### 删除

*   删除已弃用的 `fragment_cache_key` 辅助方法，推荐使用 `combined_fragment_cache_key`。
    ([Commit](https://github.com/rails/rails/commit/e70d3df7c9b05c129b0fdcca57f66eca316c5cfc))

*   删除 `ActionDispatch::TestResponse` 中已弃用的方法：
    删除`#success?`，推荐使用 `#successful?`，
    删除 `#missing?`，推荐使用 `#not_found?`，
    删除 `#error?`，推荐使用 `#server_error?`。
    ([Commit](https://github.com/rails/rails/commit/13ddc92e079e59a0b894e31bf5bb4fdecbd235d1))

### 弃用

*   弃用 `ActionDispatch::Http::ParameterFilter`，推荐使用 `ActiveSupport::ParameterFilter`。
    ([Pull Request](https://github.com/rails/rails/pull/34039))

*   弃用控制器层的 `force_ssl`，推荐使用 `config.force_ssl`。
    ([Pull Request](https://github.com/rails/rails/pull/32277))

### 重要变化

*   更改 `ActionDispatch::Response#content_type` 使之返回原生的 Content-Type 头部。
    ([Pull Request](https://github.com/rails/rails/pull/36034))

*   如果资源参数包含冒号，则引发 `ArgumentError`。
    ([Pull Request](https://github.com/rails/rails/pull/35236))

*   允许调用带有块的 `ActionDispatch::SystemTestCase.driven_by` 来定义特定的浏览器功能。
    ([Pull Request](https://github.com/rails/rails/pull/35081))

*   新增 `ActionDispatch::HostAuthorization` 中间件，以防止 DNS 重新绑定攻击。
    ([Pull Request](https://github.com/rails/rails/pull/33145))

*   允许在 `ActionController::TestCase` 中使用 `parsed_body`。
    ([Pull Request](https://github.com/rails/rails/pull/34717))

*   当多个根路由存在于同一上下文中且没有 `as:` 命名规范时，引发一个 `ArgumentError`。
    ([Pull Request](https://github.com/rails/rails/pull/34494))

*   允许使用 `#rescue_from` 处理参数解析错误。
    ([Pull Request](https://github.com/rails/rails/pull/34341))

*   新增 `ActionController::Parameters#each_value` 以遍历参数。
    ([Pull Request](https://github.com/rails/rails/pull/33979))

*   在 `send_data` 和 `send_file` 中编码 Content-Disposition 文件名。
    ([Pull Request](https://github.com/rails/rails/pull/33829))

*   暴露 `ActionController::Parameters#each_key`。
    ([Pull Request](https://github.com/rails/rails/pull/33758))

*   在已签名/加密的 cookie 中添加目的和过期元数据，以防止将 cookie 的值相互复制。
    ([Pull Request](https://github.com/rails/rails/pull/32937))

*   冲突调用 `respond_to` 会引发 `ActionController::RespondToMismatchError`。
    ([Pull Request](https://github.com/rails/rails/pull/33446))

*   为缺少请求格式的模板添加一个显式的错误页面。
    ([Pull Request](https://github.com/rails/rails/pull/29286))

*   引入 `ActionDispatch::DebugExceptions.register_interceptor`，这是一种在被渲染前，能够钩子到 DebugExceptions 并处理异常的方法。
    ([Pull Request](https://github.com/rails/rails/pull/23868))

*   每个请求仅输出一个 Content-Security-Policy 随机数头部值。
    ([Pull Request](https://github.com/rails/rails/pull/32602))

*   新增一个专门用于 Rails 默认头部配置的模块，该模块可以显式包含在控制器中。
    ([Pull Request](https://github.com/rails/rails/pull/32484))

*   新增 `#dig` 到 `ActionDispatch::Request::Session`。
    ([Pull Request](https://github.com/rails/rails/pull/32446))

Action View
-----------

有关变化详情，请参考[Changelog][action-view]。

### 删除

*   删除已弃用的 `image_alt` 辅助方法。
    ([Commit](https://github.com/rails/rails/commit/60c8a03c8d1e45e48fcb1055ba4c49ed3d5ff78f))

*   删除一个空的 `RecordTagHelper` 模块，该功能已从该模块移至 `record_tag_helper` gem。
    ([Commit](https://github.com/rails/rails/commit/5c5ddd69b1e06fb6b2bcbb021e9b8dae17e7cb31))

### 弃用

*   弃用 `ActionView::Template.finalize_compiled_template_methods` 并且没有替代。
    ([Pull Request](https://github.com/rails/rails/pull/35036))

*   弃用 `config.action_view.finalize_compiled_template_methods` 并且没有替代。
    ([Pull Request](https://github.com/rails/rails/pull/35036))

*   弃用在 `options_from_collection_for_select` 视图辅助方法中调用模型私有方法。
    ([Pull Request](https://github.com/rails/rails/pull/33547))

### 重要变化

*   开发环境下仅在文件更改时才清除 Action View 缓存，从而加快开发。
    ([Pull Request](https://github.com/rails/rails/pull/35629))

*   将所有的 Rails npm 软件包移到一个`@rails` 作用域内。
    ([Pull Request](https://github.com/rails/rails/pull/34905))

*   仅接受已注册的 MIME 类型格式。
    ([Pull Request](https://github.com/rails/rails/pull/35604), [Pull Request](https://github.com/rails/rails/pull/35753))

*   将 Allocations 添加到模板和部分渲染服务器日志输出。
    ([Pull Request](https://github.com/rails/rails/pull/34136))

*   在 `date_select` 标签中添加 `year_format` 选项，可以自定义年份名称。
    ([Pull Request](https://github.com/rails/rails/pull/32190))

*   为 `javascript_include_tag` 辅助方法添加 `nonce: true` 选项，以支持内容安全策略的自动随机数生成。
    ([Pull Request](https://github.com/rails/rails/pull/32607))

*   新增一个 `action_view.finalize_compiled_template_methods` 配置，以禁用或启用 `ActionView::Template` 析构器。
    ([Pull Request](https://github.com/rails/rails/pull/32418))

*   将 JavaScript `confirm` 调用提取到 `rails_ujs` 中去并重写该方法。
    ([Pull Request](https://github.com/rails/rails/pull/32404))

*   新增一个 `action_controller.default_enforce_utf8` 配置选项来启用
    强制 UTF-8 编码。该选项默认值为 `false`。
    ([Pull Request](https://github.com/rails/rails/pull/32125))

*   为区域设置键添加 I18n 键样式支持以提交标签。Add I18n key style support for locale keys to submit tags.
    ([Pull Request](https://github.com/rails/rails/pull/26799))

Action Mailer
-------------

有关变化详情，请参考[Changelog][action-mailer]。

### 删除

### 弃用

*   弃用 `ActionMailer::Base.receive`，推荐使用 Action Mailbox。
    ([Commit](https://github.com/rails/rails/commit/e3f832a7433a291a51c5df397dc3dd654c1858cb))

*   弃用 `DeliveryJob` 和 `Parameterized::DeliveryJob`，推荐使用 `MailDeliveryJob`。
    ([Pull Request](https://github.com/rails/rails/pull/34591))

### 重要变化

*   新增 `MailDeliveryJob` 来传递常规以及参数化的邮件。
    ([Pull Request](https://github.com/rails/rails/pull/34591))

*   允许自定义电子邮件传递任务与 Action Mailer 测试断言一起使用。
    ([Pull Request](https://github.com/rails/rails/pull/34339))

*   允许为带有块的多部分电子邮件指定模板名称，而不是仅使用动作名称。
    ([Pull Request](https://github.com/rails/rails/pull/22534))

*   在 `deliver.action_mailer` 通知的有效负载中新增 `perform_deliveries`。
    ([Pull Request](https://github.com/rails/rails/pull/33824))

*   当` perform_deliveries` 为 false 时，改进日志消息以跳过已发送的电子邮件。
    ([Pull Request](https://github.com/rails/rails/pull/33824))

*   允许调用不带块的  `assert_enqueued_email_with`。
    ([Pull Request](https://github.com/rails/rails/pull/33258))

*   在 `assert_emails` 块中执行入队的邮件传递任务。
    ([Pull Request](https://github.com/rails/rails/pull/32231))

*   允许 `ActionMailer::Base` 来取消注册观察者和拦截者。
    ([Pull Request](https://github.com/rails/rails/pull/32207))

Active Record
-------------

有关变化详情，请参考[Changelog][active-record]。

### 删除

*   删除事务对象中已弃用的 `#set_state`。
    ([Commit](https://github.com/rails/rails/commit/6c745b0c5152a4437163a67707e02f4464493983))

*   删除数据库适配器中已弃用的 `#supports_statement_cache?`。
    ([Commit](https://github.com/rails/rails/commit/5f3ed8784383fb4eb0f9959f31a9c28a991b7553))

*   删除数据库适配器中已弃用的 `#insert_fixtures`。
    ([Commit](https://github.com/rails/rails/commit/400ba786e1d154448235f5f90183e48a1043eece))

*   删除已弃用的 `ActiveRecord::ConnectionAdapters::SQLite3Adapter#valid_alter_table_type?`。
    ([Commit](https://github.com/rails/rails/commit/45b4d5f81f0c0ca72c18d0dea4a3a7b2ecc589bf))

*   删除已弃用的支持：传递列名到带代码块的 `sum`。
    ([Commit](https://github.com/rails/rails/commit/91ddb30083430622188d76eb9f29b78131df67f9))

*   删除已弃用的支持：传递列名到带代码块的 `count`。
    ([Commit](https://github.com/rails/rails/commit/67356f2034ab41305af7218f7c8b2fee2d614129))

*   删除已弃用的支持：委托关联对象缺失方法给 Arel。
    ([Commit](https://github.com/rails/rails/commit/d97980a16d76ad190042b4d8578109714e9c53d0))

*   删除已弃用的支持：委托关联对象缺失方法给该类的私有方法。
    ([Commit](https://github.com/rails/rails/commit/a7becf147afc85c354e5cfa519911a948d25fc4d))

*   删除已弃用的支持：指定 `#cache_key` 的时间戳名。
    ([Commit](https://github.com/rails/rails/commit/0bef23e630f62e38f20b5ae1d1d5dbfb087050ea))

*   删除已弃用的 `ActiveRecord::Migrator.migrations_path=`。
    ([Commit](https://github.com/rails/rails/commit/90d7842186591cae364fab3320b524e4d31a7d7d))

*   删除已弃用的 `expand_hash_conditions_for_aggregates`。
    ([Commit](https://github.com/rails/rails/commit/27b252d6a85e300c7236d034d55ec8e44f57a83e))


### 弃用

*   弃用不匹配的唯一性验证器的大小写敏感比较。
    ([Commit](https://github.com/rails/rails/commit/9def05385f1cfa41924bb93daa187615e88c95b9))

*   弃用如果接收者作用域泄漏，使用类级别的查询方法。
    ([Pull Request](https://github.com/rails/rails/pull/35280))

*   弃用 `config.active_record.sqlite3.represent_boolean_as_integer`。
    ([Commit](https://github.com/rails/rails/commit/f59b08119bc0c01a00561d38279b124abc82561b))

*   弃用传递 `migrations_paths` 给 `connection.assume_migrated_upto_version`。
    ([Commit](https://github.com/rails/rails/commit/c1b14aded27e063ead32fa911aa53163d7cfc21a))

*   弃用 `ActiveRecord::Result#to_hash`，推荐使用 `ActiveRecord::Result#to_a`。
    ([Commit](https://github.com/rails/rails/commit/16510d609c601aa7d466809f3073ec3313e08937))

*   弃用在 `DatabaseLimits` 的方法：`column_name_length`，`table_name_length`，
    `columns_per_table`，`indexes_per_table`，`columns_per_multicolumn_index`，
    `sql_query_length`，以及 `joins_per_query`。
    ([Commit](https://github.com/rails/rails/commit/e0a1235f7df0fa193c7e299a5adee88db246b44f))

*   弃用 `update_attributes`/`!`，推荐使用 `update`/`!`。
    ([Commit](https://github.com/rails/rails/commit/5645149d3a27054450bd1130ff5715504638a5f5))

### 重要变化

*   将 `sqlite3` gem 的最低版本升至 1.4。
    ([Pull Request](https://github.com/rails/rails/pull/35844))

*   新增 `rails db:prepare` 来创建一个数据库如果不存在的话，并运行迁移。
    ([Pull Request](https://github.com/rails/rails/pull/35768))

*   新增 `after_save_commit` 回调作为 `after_commit :hook, on: [ :create, :update ]` 的简写。
    ([Pull Request](https://github.com/rails/rails/pull/35804))

*   新增 `ActiveRecord::Relation#extract_associated` 来从关系中提取关联记录。
    ([Pull Request](https://github.com/rails/rails/pull/35784))

*   新增 `ActiveRecord::Relation#annotate` 来给 ActiveRecord::Relation 添加 SQL 注释。
    ([Pull Request](https://github.com/rails/rails/pull/35617))

*   新增对数据库设置优化程序提示的支持。
    ([Pull Request](https://github.com/rails/rails/pull/35615))

*   新增 `insert_all`/`insert_all!`/`upsert_all` 方法来执行批量插入。
    ([Pull Request](https://github.com/rails/rails/pull/35631))

*   新增 `rails db:seed:replant` 来针对当前环境截断每个数据库的表并加载种子数据。
    ([Pull Request](https://github.com/rails/rails/pull/34779))

*   新增 `reselect` 方法，是 `unscope(:select).select(fields)` 的简写。
    ([Pull Request](https://github.com/rails/rails/pull/33611))

*   新增所有枚举值的否定作用域。
    ([Pull Request](https://github.com/rails/rails/pull/35381))

*   新增条件删除 `#destroy_by` 和 `#delete_by`。
    ([Pull Request](https://github.com/rails/rails/pull/35316))

*   新增自动切换数据库连接的能力。
    ([Pull Request](https://github.com/rails/rails/pull/35073))

*   新增在代码块中阻止写入数据库的能力。
    ([Pull Request](https://github.com/rails/rails/pull/34505))

*   新增一个 API 来切换数据库连接以支持多数据库。
    ([Pull Request](https://github.com/rails/rails/pull/34052))

*   使有精度的时间戳作为迁移的默认值。
    ([Pull Request](https://github.com/rails/rails/pull/34970))

*   支持 `:size` 选项来改变 text 和 blob 在 MySQL 中的大小。
    ([Pull Request](https://github.com/rails/rails/pull/35071))

*   对于 `dependent: :nullify` 策略的多态关联，将外键和外键类型列都设置为 NULL。
    ([Pull Request](https://github.com/rails/rails/pull/28078))

*   允许将 `ActionController::Parameters` 的实例作为参数传递给 `ActiveRecord::Relation#exists?`。
    ([Pull Request](https://github.com/rails/rails/pull/34891))

*   新增 Ruby 2.6 中引入的无穷范围在 `#where` 的支持。
    ([Pull Request](https://github.com/rails/rails/pull/34906))

*   使 `ROW_FORMAT=DYNAMIC` 成为 MySQL 创建表的默认选项。
    ([Pull Request](https://github.com/rails/rails/pull/34742))

*   新增停止通过 `ActiveRecord.enum` 生成作用域的能力。
    ([Pull Request](https://github.com/rails/rails/pull/34605))

*   使列的隐式排序可配置。
    ([Pull Request](https://github.com/rails/rails/pull/34480))

*   将最低 PostgreSQL 版本提高到 9.3，放弃 9.1 和 9.2 的支持。
    ([Pull Request](https://github.com/rails/rails/pull/34520))

*   使枚举类型的值冻结，当尝试修改它们时，引发一个错误。
    ([Pull Request](https://github.com/rails/rails/pull/34517))

*   使 SQL 的 `ActiveRecord::StatementInvalid` 错误成为自己的错误属性，并使 SQL 绑定作为单独的错误属性。
    ([Pull Request](https://github.com/rails/rails/pull/34468))

*   新增一个 `:if_not_exists` 选项到 `create_table`。
    ([Pull Request](https://github.com/rails/rails/pull/31382))

*   在 `rails db:schema:cache:dump`和 `rails db:schema:cache:clear` 添加对多数据库的支持。
    ([Pull Request](https://github.com/rails/rails/pull/34181))

*   在 `ActiveRecord::Base.connected_to` 的数据库哈希参数中添加对哈希和 url 配置的支持。
    ([Pull Request](https://github.com/rails/rails/pull/34196))

*   添加对 MySQL 的默认表达式和表达式索引的支持。
    ([Pull Request](https://github.com/rails/rails/pull/34307))

*   为 `change_table` 迁移辅助方法添加一个 `index` 选项。
    ([Pull Request](https://github.com/rails/rails/pull/23593))

*   修复 `transaction` 还原迁移的问题。 以前，在还原迁移中的 `transaction` 内部的命令未还原。此更改可解决此问题。
    ([Pull Request](https://github.com/rails/rails/pull/31604))

*   允许 `ActiveRecord::Base.configurations=` 由一个符号哈希设置。
    ([Pull Request](https://github.com/rails/rails/pull/33968))

*   修复计数器缓存，使其仅在实际保存记录时才更新。
    ([Pull Request](https://github.com/rails/rails/pull/33913))

*   添加对 SQLite 适配器的表达式索引支持。
    ([Pull Request](https://github.com/rails/rails/pull/33874))

*   允许子类为关联记录重新定义 autosave 回调。
    ([Pull Request](https://github.com/rails/rails/pull/33378))

*   将最低 MySQL 版本提升至 5.5.8。
    ([Pull Request](https://github.com/rails/rails/pull/33853))

*   默认情况下，在 MySQL 中使用 utf8mb4 字符集。
    ([Pull Request](https://github.com/rails/rails/pull/33608))

*   添加了在` #inspect` 中过滤掉敏感数据的功能。
    ([Pull Request](https://github.com/rails/rails/pull/33756), [Pull Request](https://github.com/rails/rails/pull/34208))

*   更改 `ActiveRecord::Base.configurations` 以返回对象而不是哈希。
    ([Pull Request](https://github.com/rails/rails/pull/33637))

*   添加数据库配置以禁用 advisory 锁。
    ([Pull Request](https://github.com/rails/rails/pull/33691))

*   更新 SQLite3 适配器的 `alter_table` 方法以还原外键。
    ([Pull Request](https://github.com/rails/rails/pull/33585))

*   允许 `remove_foreign_key` 的 `:to_table` 选项是可逆的。
    ([Pull Request](https://github.com/rails/rails/pull/33530))

*   使用指定的精度修复 MySQL 时间类型的默认值。
    ([Pull Request](https://github.com/rails/rails/pull/33280))

*   修复 `touch` 选项，使其与 `Persistence＃touch` 方法保持一致。
    ([Pull Request](https://github.com/rails/rails/pull/33107))

*   现在迁移中的重复列定义会引发异常。
    ([Pull Request](https://github.com/rails/rails/pull/33029))

*   将最低 SQLite 版本提高到 3.8。
    ([Pull Request](https://github.com/rails/rails/pull/32923))

*   修复父记录，使其不会与重复的子记录一起保存。
    ([Pull Request](https://github.com/rails/rails/pull/32952))

*   确保 `Associations::CollectionAssociation#size` 和 `Associations::CollectionAssociation#empty?` 使用已载入的关联 id（如果关联 id 存在的话）。
    ([Pull Request](https://github.com/rails/rails/pull/32617))

*   当并非所有记录都具有请求的关联时，为多态关联的预加载关联添加支持。
    ([Commit](https://github.com/rails/rails/commit/75ef18c67c29b1b51314b6c8a963cee53394080b))

*   在 `ActiveRecord::Relation` 中添加 `touch_all` 方法。
    ([Pull Request](https://github.com/rails/rails/pull/31513))

*   添加 `ActiveRecord::Base.base_class?` 谓词。
    ([Pull Request](https://github.com/rails/rails/pull/32417))

*   在 `ActiveRecord::Store.store_accessor` 中添加自定义前缀/后缀选项。
    ([Pull Request](https://github.com/rails/rails/pull/32306))

*   通过依赖数据库中的唯一约束，在 `ActiveRecord::Base.find_or_create_by`/`!` 中添加 `ActiveRecord::Base.create_or_find_by`/`!` 来处理 SELECT/INSERT 竞争条件。
    ([Pull Request](https://github.com/rails/rails/pull/31989))

*   添加 `Relation＃pick` 作为单个值 pluck 的简写。
    ([Pull Request](https://github.com/rails/rails/pull/31941))

Active Storage
--------------

有关变化详情，请参考[Changelog][active-storage]。

### 删除

### 弃用

*   弃用 `config.active_storage.queue`，推荐使用 `config.active_storage.queues.analysis` 和 `config.active_storage.queues.purge`。
    ([Pull Request](https://github.com/rails/rails/pull/34838))

*   弃用 `ActiveStorage::Downloading`，推荐使用 `ActiveStorage::Blob#open`。
    ([Commit](https://github.com/rails/rails/commit/ee21b7c2eb64def8f00887a9fafbd77b85f464f1))

*   弃用直接使用 `mini_magick` 生成图片 variants，推荐使用  `image_processing`。
    ([Commit](https://github.com/rails/rails/commit/697f4a93ad386f9fb7795f0ba68f815f16ebad0f))

*   弃用 Active Storage's ImageProcessing transformer 中的 `:combine_options` 并且没有替代。
    ([Commit](https://github.com/rails/rails/commit/697f4a93ad386f9fb7795f0ba68f815f16ebad0f))

### 重要变化

*   新增生成 BMP 图片 variants 的支持。
    ([Pull Request](https://github.com/rails/rails/pull/36051))

*   新增生成 TIFF 图片 variants 的支持。
    ([Pull Request](https://github.com/rails/rails/pull/34824))

*   新增生成渐进式 JPEG 图片 variants 的支持。
    ([Pull Request](https://github.com/rails/rails/pull/34455))

*   新增 `ActiveStorage.routes_prefix` 来配置 Active Storage 生成的路由。
    ([Pull Request](https://github.com/rails/rails/pull/33883))

*   当被请求文件不存在于存储服务中，在 `ActiveStorage::DiskController#show` 生成一个 404 Not Found 响应。
    ([Pull Request](https://github.com/rails/rails/pull/33666))

*   当缺少 `ActiveStorage::Blob#download` 和 `ActiveStorage::Blob#open` 所请求的文件时，引发 `ActiveStorage::FileNotFoundError`。
    ([Pull Request](https://github.com/rails/rails/pull/33666))

*   添加一个通用的 `ActiveStorage::Error` 类，Active Storage 异常继承自该类。
    ([Commit](https://github.com/rails/rails/commit/18425b837149bc0d50f8d5349e1091a623762d6b))

*   记录 save（之前是分配）时，将分配给记录的上传的文件持久保存到存储中。
    ([Pull Request](https://github.com/rails/rails/pull/33303))

*   可以选择替换现有文件，而不是在分配给 attachments 集合时添加它们（如在 `@user.update!(images: [ … ])` 中）。
    使用 `config.active_storage.replace_on_assign_to_many` 来控制这种行为。
    ([Pull Request](https://github.com/rails/rails/pull/33303),
     [Pull Request](https://github.com/rails/rails/pull/36716))

*   使用现有的 Active Record 反射机制来添加定义的 attachments 的反射功能。
    ([Pull Request](https://github.com/rails/rails/pull/33018))

*   新增 `ActiveStorage::Blob#open`，它将 blob 下载到磁盘上的临时文件并生成该临时文件。
    ([Commit](https://github.com/rails/rails/commit/ee21b7c2eb64def8f00887a9fafbd77b85f464f1))

*   支持从 Google Cloud Storage 流式下载。需要 `google-cloud-storage` gem 的1.11+ 版本。
    ([Pull Request](https://github.com/rails/rails/pull/32788))

*   将 `image_processing` gem 用于 Active Storage variants。这将直接替换为 `mini_magick`。
    ([Pull Request](https://github.com/rails/rails/pull/32471))

Active Model
------------

有关变化详情，请参考[Changelog][active-model]。

### 删除

### 弃用

### 重要变化

*   新增一个配置选项来自定义 `ActiveModel::Errors#full_message` 的格式。
    ([Pull Request](https://github.com/rails/rails/pull/32956))

*   新增配置 `has_secure_password` 属性名的能力。
    ([Pull Request](https://github.com/rails/rails/pull/26764))

*   在 `ActiveModel::Errors` 中新增 `#slice!` 方法。
    ([Pull Request](https://github.com/rails/rails/pull/34489))

*   新增 `ActiveModel::Errors#of_kind?` 来检查特定错误是否存在。
    ([Pull Request](https://github.com/rails/rails/pull/34866))

*   修复 `ActiveModel::Serializers::JSON#as_json` 方法的时间戳。
    ([Pull Request](https://github.com/rails/rails/pull/31503))

*   修复数值验证程序，以便在类型转换之前仍使用值（Active Record 除外）。
    ([Pull Request](https://github.com/rails/rails/pull/33654))

*   通过在验证的两端强制转换为 `BigDecimal`，修复 `BigDecimal` 和 `Float` 的数值相等性验证。
    ([Pull Request](https://github.com/rails/rails/pull/32852))

*   修复转换多参数时间哈希值时的年份值。
    ([Pull Request](https://github.com/rails/rails/pull/34990))

*   将布尔属性上的伪造布尔符号键入强制转换为 false。
    ([Pull Request](https://github.com/rails/rails/pull/35794))

*   在为 `ActiveModel::Type::Date` 转换 `value_from_multiparameter_assignment` 中的参数时，返回正确的日期。
    ([Pull Request](https://github.com/rails/rails/pull/29651))

*   在获取错误转换时，先回到父语言环境，然后再回到 `:errors` 名称空间。
    ([Pull Request](https://github.com/rails/rails/pull/35424))

Active Support
--------------

Please refer to the [Changelog][active-support] for detailed changes.

### Removals

*   Remove deprecated `#acronym_regex` method from `Inflections`.
    ([Commit](https://github.com/rails/rails/commit/0ce67d3cd6d1b7b9576b07fecae3dd5b422a5689))

*   Remove deprecated `Module#reachable?` method.
    ([Commit](https://github.com/rails/rails/commit/6eb1d56a333fd2015610d31793ed6281acd66551))

*   Remove `` Kernel#` `` without any replacement.
    ([Pull Request](https://github.com/rails/rails/pull/31253))

### Deprecations

*   Deprecate using negative integer arguments for `String#first` and
    `String#last`.
    ([Pull Request](https://github.com/rails/rails/pull/33058))

*   Deprecate `ActiveSupport::Multibyte::Unicode#downcase/upcase/swapcase`
    in favor of `String#downcase/upcase/swapcase`.
    ([Pull Request](https://github.com/rails/rails/pull/34123))

*   Deprecate `ActiveSupport::Multibyte::Unicode#normalize`
    and `ActiveSupport::Multibyte::Chars#normalize` in favor of
    `String#unicode_normalize`.
    ([Pull Request](https://github.com/rails/rails/pull/34202))

*   Deprecate `ActiveSupport::Multibyte::Chars.consumes?` in favor of
    `String#is_utf8?`.
    ([Pull Request](https://github.com/rails/rails/pull/34215))

*   Deprecate `ActiveSupport::Multibyte::Unicode#pack_graphemes(array)`
    and `ActiveSupport::Multibyte::Unicode#unpack_graphemes(string)`
    in favor of `array.flatten.pack("U*")` and `string.scan(/\X/).map(&:codepoints)`,
    respectively.
    ([Pull Request](https://github.com/rails/rails/pull/34254))

### Notable changes

*   Add support for parallel testing.
    ([Pull Request](https://github.com/rails/rails/pull/31900))

*   Make sure that `String#strip_heredoc` preserves frozen-ness of strings.
    ([Pull Request](https://github.com/rails/rails/pull/32037))

*   Add `String#truncate_bytes` to truncate a string to a maximum bytesize
    without breaking multibyte characters or grapheme clusters.
    ([Pull Request](https://github.com/rails/rails/pull/27319))

*   Add `private` option to `delegate` method in order to delegate to
    private methods. This option accepts `true/false` as the value.
    ([Pull Request](https://github.com/rails/rails/pull/31944))

*   Add support for translations through I18n for `ActiveSupport::Inflector#ordinal`
    and `ActiveSupport::Inflector#ordinalize`.
    ([Pull Request](https://github.com/rails/rails/pull/32168))

*   Add `before?` and `after?` methods to `Date`, `DateTime`,
    `Time`, and `TimeWithZone`.
    ([Pull Request](https://github.com/rails/rails/pull/32185))

*   Fix bug where `URI.unescape` would fail with mixed Unicode/escaped character
    input.
    ([Pull Request](https://github.com/rails/rails/pull/32183))

*   Fix bug where `ActiveSupport::Cache` would massively inflate the storage
    size when compression was enabled.
    ([Pull Request](https://github.com/rails/rails/pull/32539))

*   Redis cache store: `delete_matched` no longer blocks the Redis server.
    ([Pull Request](https://github.com/rails/rails/pull/32614))

*   Fix bug where `ActiveSupport::TimeZone.all` would fail when tzinfo data for
    any timezone defined in `ActiveSupport::TimeZone::MAPPING` was missing.
    ([Pull Request](https://github.com/rails/rails/pull/32613))

*   Add `Enumerable#index_with` which allows creating a hash from an enumerable
    with the value from a passed block or a default argument.
    ([Pull Request](https://github.com/rails/rails/pull/32523))

*   Allow `Range#===` and `Range#cover?` methods to work with `Range` argument.
    ([Pull Request](https://github.com/rails/rails/pull/32938))

*   Support key expiry in `increment/decrement` operations of RedisCacheStore.
    ([Pull Request](https://github.com/rails/rails/pull/33254))

*   Add cpu time, idle time, and allocations features to log subscriber events.
    ([Pull Request](https://github.com/rails/rails/pull/33449))

*   Add support for event object to the Active Support notification system.
    ([Pull Request](https://github.com/rails/rails/pull/33451))

*   Add support for not caching `nil` entries by introducing new option `skip_nil`
    for `ActiveSupport::Cache#fetch`.
    ([Pull Request](https://github.com/rails/rails/pull/25437))

*   Add `Array#extract!` method which removes and returns the elements for which
    block returns a true value.
    ([Pull Request](https://github.com/rails/rails/pull/33137))

*   Keep an HTML-safe string HTML-safe after slicing.
    ([Pull Request](https://github.com/rails/rails/pull/33808))

*   Add support for tracing constant autoloads via logging.
    ([Commit](https://github.com/rails/rails/commit/c03bba4f1f03bad7dc034af555b7f2b329cf76f5))

*   Define `unfreeze_time` as an alias of `travel_back`.
    ([Pull Request](https://github.com/rails/rails/pull/33813))

*   Change `ActiveSupport::TaggedLogging.new` to return a new logger instance
    instead of mutating the one received as argument.
    ([Pull Request](https://github.com/rails/rails/pull/27792))

*   Treat `#delete_prefix`, `#delete_suffix` and `#unicode_normalize` methods
    as non HTML-safe methods.
    ([Pull Request](https://github.com/rails/rails/pull/33990))

*   Fix bug where `#without` for `ActiveSupport::HashWithIndifferentAccess`
    would fail with symbol arguments.
    ([Pull Request](https://github.com/rails/rails/pull/34012))

*   Rename `Module#parent`, `Module#parents`, and `Module#parent_name` to
    `module_parent`, `module_parents`, and `module_parent_name`.
    ([Pull Request](https://github.com/rails/rails/pull/34051))

*   Add `ActiveSupport::ParameterFilter`.
    ([Pull Request](https://github.com/rails/rails/pull/34039))

*   Fix issue where duration was being rounded to a full second when a float
    was added to the duration.
    ([Pull Request](https://github.com/rails/rails/pull/34135))

*   Make `#to_options` an alias for `#symbolize_keys` in
    `ActiveSupport::HashWithIndifferentAccess`.
    ([Pull Request](https://github.com/rails/rails/pull/34360))

*   Don't raise an exception anymore if the same block is included multiple times
    for a Concern.
    ([Pull Request](https://github.com/rails/rails/pull/34553))

*   Preserve key order passed to `ActiveSupport::CacheStore#fetch_multi`.
    ([Pull Request](https://github.com/rails/rails/pull/34700))

*   Fix `String#safe_constantize` to not throw a `LoadError` for incorrectly
    cased constant references.
    ([Pull Request](https://github.com/rails/rails/pull/34892))

*   Add `Hash#deep_transform_values` and `Hash#deep_transform_values!`.
    ([Commit](https://github.com/rails/rails/commit/b8dc06b8fdc16874160f61dcf58743fcc10e57db))

*   Add `ActiveSupport::HashWithIndifferentAccess#assoc`.
    ([Pull Request](https://github.com/rails/rails/pull/35080))

*   Add `before_reset` callback to `CurrentAttributes` and define
    `after_reset` as an alias of `resets` for symmetry.
    ([Pull Request](https://github.com/rails/rails/pull/35063))

*   Revise `ActiveSupport::Notifications.unsubscribe` to correctly
    handle Regex or other multiple-pattern subscribers.
    ([Pull Request](https://github.com/rails/rails/pull/32861))

*   Add new autoloading mechanism using Zeitwerk.
    ([Commit](https://github.com/rails/rails/commit/e53430fa9af239e21e11548499d814f540d421e5))

*   Add `Array#including` and `Enumerable#including` to conveniently enlarge
    a collection.
    ([Commit](https://github.com/rails/rails/commit/bfaa3091c3c32b5980a614ef0f7b39cbf83f6db3))

*   Rename `Array#without` and `Enumerable#without` to `Array#excluding`
    and `Enumerable#excluding`. Old method names are retained as aliases.
    ([Commit](https://github.com/rails/rails/commit/bfaa3091c3c32b5980a614ef0f7b39cbf83f6db3))

*   Add support for supplying `locale` to `transliterate` and `parameterize`.
    ([Pull Request](https://github.com/rails/rails/pull/35571))

*   Fix `Time#advance` to work with dates before 1001-03-07.
    ([Pull Request](https://github.com/rails/rails/pull/35659))

*   Update `ActiveSupport::Notifications::Instrumenter#instrument` to allow
    not passing block.
    ([Pull Request](https://github.com/rails/rails/pull/35705))

*   Use weak references in descendants tracker to allow anonymous subclasses to
    be garbage collected.
    ([Pull Request](https://github.com/rails/rails/pull/31442))

*   Calling test methods with `with_info_handler` method to allow minitest-hooks
    plugin to work.
    ([Commit](https://github.com/rails/rails/commit/758ba117a008b6ea2d3b92c53b6a7a8d7ccbca69))

*   Preserve `html_safe?` status on `ActiveSupport::SafeBuffer#*`.
    ([Pull Request](https://github.com/rails/rails/pull/36012))

Active Job
----------

Please refer to the [Changelog][active-job] for detailed changes.

### Removals

*   Remove support for Qu gem.
    ([Pull Request](https://github.com/rails/rails/pull/32300))

### Deprecations

### Notable changes

*   Add support for custom serializers for Active Job arguments.
    ([Pull Request](https://github.com/rails/rails/pull/30941))

*   Add support for executing Active Jobs in the timezone in which
    they were enqueued.
    ([Pull Request](https://github.com/rails/rails/pull/32085))

*   Allow passing multiple exceptions to `retry_on`/`discard_on`.
    ([Commit](https://github.com/rails/rails/commit/3110caecbebdad7300daaf26bfdff39efda99e25))

*   Allow calling `assert_enqueued_with` and `assert_enqueued_email_with` without a block.
    ([Pull Request](https://github.com/rails/rails/pull/33258))

*   Wrap the notifications for `enqueue` and `enqueue_at` in the `around_enqueue`
    callback instead of `after_enqueue` callback.
    ([Pull Request](https://github.com/rails/rails/pull/33171))

*   Allow calling `perform_enqueued_jobs` without a block.
    ([Pull Request](https://github.com/rails/rails/pull/33626))

*   Allow calling `assert_performed_with` without a block.
    ([Pull Request](https://github.com/rails/rails/pull/33635))

*   Add `:queue` option to job assertions and helpers.
    ([Pull Request](https://github.com/rails/rails/pull/33635))

*   Add hooks to Active Job around retries and discards.
    ([Pull Request](https://github.com/rails/rails/pull/33751))

*   Add a way to test for subset of arguments when performing jobs.
    ([Pull Request](https://github.com/rails/rails/pull/33995))

*   Include deserialized arguments in jobs returned by Active Job
    test helpers.
    ([Pull Request](https://github.com/rails/rails/pull/34204))

*   Allow Active Job assertion helpers to accept Proc for `only`
    keyword.
    ([Pull Request](https://github.com/rails/rails/pull/34339))

*   Drop microseconds and nanoseconds from the job arguments in assertion helpers.
    ([Pull Request](https://github.com/rails/rails/pull/35713))

Ruby on Rails Guides
--------------------

Please refer to the [Changelog][guides] for detailed changes.

### Notable changes

*   Add Multiple Databases with Active Record guide.
    ([Pull Request](https://github.com/rails/rails/pull/36389))

*   Add a section about troubleshooting of autoloading constants.
    ([Commit](https://github.com/rails/rails/commit/c03bba4f1f03bad7dc034af555b7f2b329cf76f5))

*   Add Action Mailbox Basics guide.
    ([Pull Request](https://github.com/rails/rails/pull/34812))

*   Add Action Text Overview guide.
    ([Pull Request](https://github.com/rails/rails/pull/34878))

Credits
-------

See the
[full list of contributors to Rails](https://contributors.rubyonrails.org/)
for the many people who spent many hours making Rails, the stable and robust
framework it is. Kudos to all of them.

[railties]:       https://github.com/rails/rails/blob/6-0-stable/railties/CHANGELOG.md
[action-pack]:    https://github.com/rails/rails/blob/6-0-stable/actionpack/CHANGELOG.md
[action-view]:    https://github.com/rails/rails/blob/6-0-stable/actionview/CHANGELOG.md
[action-mailer]:  https://github.com/rails/rails/blob/6-0-stable/actionmailer/CHANGELOG.md
[action-cable]:   https://github.com/rails/rails/blob/6-0-stable/actioncable/CHANGELOG.md
[active-record]:  https://github.com/rails/rails/blob/6-0-stable/activerecord/CHANGELOG.md
[active-storage]: https://github.com/rails/rails/blob/6-0-stable/activestorage/CHANGELOG.md
[active-model]:   https://github.com/rails/rails/blob/6-0-stable/activemodel/CHANGELOG.md
[active-support]: https://github.com/rails/rails/blob/6-0-stable/activesupport/CHANGELOG.md
[active-job]:     https://github.com/rails/rails/blob/6-0-stable/activejob/CHANGELOG.md
[guides]:         https://github.com/rails/rails/blob/6-0-stable/guides/CHANGELOG.md
