**不要在 GITHUB 上阅读此文件，指南被发布在 https://guides.rubyonrails.org。**

Ruby on Rails 6.1 发行说明
===============================

Rails 6.1 中的亮点：

* 数据库连接的独立切换
* 水平拆分
* 严格加载关联对象
* 委托类型
* 异步销毁关联对象

这些发行说明仅涵盖主要变化。
要了解各种错误修复和变化，请参阅更改日志或查看 GitHub 上的[提交列表](https://github.com/rails/rails/commits/master)。

--------------------------------------------------------------------------------

升级到 Rails 6.1
----------------------

如果要升级现有的应用程序，那么应该在升级之前拥有良好的测试覆盖。
以防万一，应该首先升级到 Rails 6.0，并确保应用程序仍按预期运行，然后再尝试更新到 Rails 6.1。
[升级 Ruby on Rails](upgrading_ruby_on_rails.html#upgrading-from-rails-6-0-to-rails-6-1) 指南中提供了升级时需要注意的事项列表。

主要功能
--------------

### 数据库连接的独立切换

Rails 6.1 使你能够[独立切换数据库连接]（https://github.com/rails/rails/pull/40370。在 6.0 中，如果你切换到 `reading` 角色，则所有数据库连接也都切换到了 `reading` 角色。现在 6.1 中，如果在配置中将 `legacy_connection_handling` 设置为 `false`，Rails 将允许你通过在相应的抽象类上调用 `connected_to` 来切换单个数据库的连接。

### 水平拆分

Rails 6.0 提供了在功能上对数据库进行拆分（多个分区，不同 schema）的功能，但无法支持水平拆分（相同模式，多个分区）。Rails 无法支持水平拆分，因为 Active Record 中的模型每个类每个角色只能有一个连接。现在此问题已解决，可以使用 Rails 的[水平拆分](https://github.com/rails/rails/pull/38531)。

### 严格加载关联对象

[严格加载关联对象](https://github.com/rails/rails/pull/37400) 确保所有的关联会及早加载，并在它们发生之前阻止 N+1。

### 委托类型

[委托类型](https://github.com/rails/rails/pull/39341) 是单表继承的替代方法。允许父类成为由其自己的表表示的具体类，这有助于表示类层次结构。每个子类都有自己的表以用于其他属性。

### 异步销毁关联对象

[异步销毁关联对象](https://github.com/rails/rails/pull/40157) 增加了应用成语在后台任务中 `destroy` 关联对象的功能。这有助于避免删除数据时的超时问题以及其他性能问题。

Railties
--------

有关变化详情，请参考 [Changelog][railties]。

### 删除

*   删除已弃用的 `rake notes` 任务。

*   删除 `rails dbconsole` 命令中已弃用的 `connection` 选项。

*   删除 `rails notes` 中已弃用的 `SOURCE_ANNOTATION_DIRECTORIES` 环境变量。

*   删除 `rails server`中已弃用的 `server` 参数。

*   删除已弃用的支持：使用 `HOST` 环境变量来指定服务器 IP。

*   删除已弃用的 `rake dev:cache` 任务。

*   删除已弃用的 `rake routes` 任务。

*   删除已弃用的 `rake initializers` 任务。

### 弃用

### 重要变化

Action Cable
------------

有关变化详情，请参考 [Changelog][action-cable]。

### 删除

### 弃用

### 重要变化

Action Pack
-----------

有关变化详情，请参考 [Changelog][action-pack]。

### 删除

*   删除已弃用的 `ActionDispatch::Http::ParameterFilter`。

*   删除控制器层中已弃用的 `force_ssl`。

### 弃用

*   弃用 `config.action_dispatch.return_only_media_type_on_content_type`。

### 重要变化

*   改变 `ActionDispatch::Response#content_type` 来返回完整的 Content-Type 头部。

Action View
-----------

有关变化详情，请参考 [Changelog][action-view]。

### 删除

*   删除 `ActionView::Template::Handlers::ERB` 中已弃用的 `escape_whitelist`。

*   删除 `ActionView::Resolver` 中已弃用的 `find_all_anywhere`。

*   删除 `ActionView::Template::HTML` 中已弃用的 `formats`。

*   删除 `ActionView::Template::RawFile` 中已弃用的 `formats`。

*   删除 `ActionView::Template::Text` 中已弃用的 `formats`。

*   删除 `ActionView::PathSet` 中已弃用的 `find_file`。

*   删除 `ActionView::LookupContext` 中已弃用的 `rendered_format`。

*   删除 `ActionView::ViewPaths` 中已弃用的 `find_file`。

*   删除已弃用的支持：将不是 `ActionView::LookupContext` 的对象作为 `ActionView::Base#initialize` 中的第一个参数传递。

*   删除 `ActionView::Base#initialize` 中已弃用的 `format` 参数。

*   删除已弃用的 `ActionView::Template#refresh`。

*   删除已弃用的 `ActionView::Template#original_encoding`。

*   删除已弃用的 `ActionView::Template#variants`。

*   删除已弃用的 `ActionView::Template#formats`。

*   删除已弃用的 `ActionView::Template#virtual_path=`。

*   删除已弃用的 `ActionView::Template#updated_at`。

*   删除 `ActionView::Template#initialize` 中需要的已弃用的 `updated_at` 参数。

*   删除已弃用的 `ActionView::Template.finalize_compiled_template_methods`。

*   删除已弃用的 `config.action_view.finalize_compiled_template_methods`。

*   删除已弃用的支持：调用有代码块的 `ActionView::ViewPaths#with_fallback`。

*   删除已弃用的支持：传绝对路径给 `render template:`。

*   删除已弃用的支持：传相对路径给 `render file:`。

*   删除已弃用的支持：模版处理不接受两个参数。

*   删除 `ActionView::Template::PathResolver` 中已弃用的模式参数。

*   删除已弃用的支持：在某些视图辅助方法中调用对象的私有方法。

### 弃用

### 重要变化

*   要求 `ActionView::Base` 子类实现 `#compiled_method_container`。

*   现在 `ActionView::Template#initialize` 必须传 `locals` 参数。

Action Mailer
-------------

有关变化详情，请参考 [Changelog][action-mailer]。

### 删除

*   删除已弃用的 `ActionMailer::Base.receive`，推荐使用 [Action Mailbox](https://github.com/rails/rails/tree/master/actionmailbox)。

### 弃用

### 重要变化

Active Record
-------------

有关变化详情，请参考 [Changelog][active-record]。

### 删除

*   删除 `ActiveRecord::ConnectionAdapters::DatabaseLimits` 中已弃用的方法。

    `column_name_length`
    `table_name_length`
    `columns_per_table`
    `indexes_per_table`
    `columns_per_multicolumn_index`
    `sql_query_length`
    `joins_per_query`

*   删除已弃用的 `ActiveRecord::ConnectionAdapters::AbstractAdapter#supports_multi_insert?`。

*   删除已弃用的 `ActiveRecord::ConnectionAdapters::AbstractAdapter#supports_foreign_keys_in_create?`。

*   删除已弃用的 `ActiveRecord::ConnectionAdapters::PostgreSQLAdapter#supports_ranges?`。

*   删除已弃用的 `ActiveRecord::Base#update_attributes` 和 `ActiveRecord::Base#update_attributes!`。

*   删除 `ActiveRecord::ConnectionAdapter::SchemaStatements#assume_migrated_upto_version` 中已弃用的 `migrations_path` 参数。

*   删除已弃用的 `config.active_record.sqlite3.represent_boolean_as_integer`。

*   删除 `ActiveRecord::DatabaseConfigurations` 中已弃用的方法。

    `fetch`
    `each`
    `first`
    `values`
    `[]=`

*   删除已弃用的 `ActiveRecord::Result#to_hash` 方法。

*   删除已弃用的支持：在 `ActiveRecord::Relation` 系列方法中使用不安全的原始SQL。

### 弃用

*   弃用 `ActiveRecord::Base.allow_unsafe_raw_sql`。

### 重要变化

*   MySQL：唯一性验证器现在遵守默认的数据库排序规则，默认情况下不再强制执行区分大小写的比较。

*   `relation.create` 不再将范围泄漏到类级别的初始化块以及回调中的的查询方法。

    之前：

        User.where(name: "John").create do |john|
          User.find_by(name: "David") # => nil
        end

    之后：

        User.where(name: "John").create do |john|
          User.find_by(name: "David") # => #<User name: "David", ...>
        end

*   命名作用域链不再将作用域泄漏给类级别的查询方法。

        class User < ActiveRecord::Base
          scope :david, -> { User.where(name: "David") }
        end

    之前：

        User.where(name: "John").david
        # SELECT * FROM users WHERE name = 'John' AND name = 'David'

    之后：

        User.where(name: "John").david
        # SELECT * FROM users WHERE name = 'David'

*   `where.not` 现在生成 NAND 谓词，而不是 NOR。

     之前：

         User.where.not(name: "Jon", role: "admin")
         # SELECT * FROM users WHERE name != 'Jon' AND role != 'admin'

     之后：

         User.where.not(name: "Jon", role: "admin")
         # SELECT * FROM users WHERE NOT (name == 'Jon' AND role == 'admin')

Active Storage
--------------

有关变化详情，请参考 [Changelog][active-storage]。

### 删除

*   删除已弃用的支持：将 `:combine_options` 操作传递给 `ActiveStorage::Transformers::ImageProcessing`。

*   删除已弃用的 `ActiveStorage::Transformers::MiniMagickTransformer`。

*   删除已弃用的 `config.active_storage.queue`。

*   删除已弃用的 `ActiveStorage::Downloading`。

### 弃用
*   删除已弃用的 `Blob.create_after_upload`，推荐使用 `Blob.create_and_upload`。
    ([Pull Request](https://github.com/rails/rails/pull/34827))

### 重要变化

*  新增 `Blob.create_and_upload` 创建一个新的 blob，并将给定的 `io` 上传到服务中。
    ([Pull Request](https://github.com/rails/rails/pull/34827))

Active Model
------------

有关变化详情，请参考 [Changelog][active-model]。

### 删除

### 弃用

### 重要变化

*   Active Model 的错误现在是带有接口的对象，该接口使应用程序可以更轻松地处理模型引发的错误并与之交互。
    [此功能](https://github.com/rails/rails/pull/32313) 包括查询接口，可以进行更精确的测试，并可以获取错误详细信息。

Active Support
--------------

有关变化详情，请参考 [Changelog][active-support]。

### 删除

*   删除已弃用的回退：当 `config.i18n.fallbacks` 为空时会使用 `I18n.default_local`。

*   删除已弃用的 `LoggerSilence` 常量。

*   删除已弃用的 `ActiveSupport::LoggerThreadSafeLevel#after_initialize`。

*   删除已弃用的 `Module#parent_name`, `Module#parent` 和 `Module#parents`。

*   删除已弃用的 `active_support/core_ext/module/reachable` 文件。

*   删除已弃用的 `active_support/core_ext/numeric/inquiry` 文件。

*   删除已弃用的 `active_support/core_ext/array/prepend_and_append` 文件。

*   删除已弃用的 `active_support/core_ext/hash/compact` 文件。

*   删除已弃用的 `active_support/core_ext/hash/transform_values` 文件。

*   删除已弃用的 `active_support/core_ext/range/include_range` 文件。

*   删除已弃用的 `ActiveSupport::Multibyte::Chars#consumes?` 和 `ActiveSupport::Multibyte::Chars#normalize`。

*   删除已弃用的 `ActiveSupport::Multibyte::Unicode.pack_graphemes`，
    `ActiveSupport::Multibyte::Unicode.unpack_graphemes`，
    `ActiveSupport::Multibyte::Unicode.normalize`，
    `ActiveSupport::Multibyte::Unicode.downcase`，
    `ActiveSupport::Multibyte::Unicode.upcase` 和 `ActiveSupport::Multibyte::Unicode.swapcase`。

*   删除已弃用的 `ActiveSupport::Notifications::Instrumenter#end=`。

### 弃用

*   弃用 `ActiveSupport::Multibyte::Unicode.default_normalization_form`。

### 重要变化

Active Job
----------

有关变化详情，请参考 [Changelog][active-job]。

### 删除

### 弃用

*   弃用 `config.active_job.return_false_on_aborted_enqueue`。

### 重要变化

*   后台任务入队终止时返回 `false`。

Action Text
----------

有关变化详情，请参考 [Changelog][action-text]。

### 删除

### 弃用

### 重要变化

*   新增通过在富文本属性名称之后添加 `?` 来确认富文本内容存在。
    ([Pull Request](https://github.com/rails/rails/pull/37951))

*   新增 `fill_in_rich_text_area` 系统测试用例辅助方法，以找到 Trix 编辑器并使用给定的 HTML 内容填充它。
    ([Pull Request](https://github.com/rails/rails/pull/35885))

*   新增 `ActionText::FixtureSet.attachment` 使得可以在数据库固件中生成 `<action-text-attachment>` 元素。
    ([Pull Request](https://github.com/rails/rails/pull/40289))

Action Mailbox
----------

有关变化详情，请参考 [Changelog][action-mailbox]。

### 删除

### 弃用

### 重要变化

Ruby on Rails 指南
--------------------

有关变化详情，请参考 [Changelog][guides]。

### 重要变化

工作人员
-------

请参阅 [Rails 贡献者的完整列表](https://contributors.rubyonrails.org/)
来查看花了很多时间来编写稳定且强大的 Rails 框架的成员。对所有成员表示敬意。

[railties]:       https://github.com/rails/rails/blob/master/railties/CHANGELOG.md
[action-pack]:    https://github.com/rails/rails/blob/master/actionpack/CHANGELOG.md
[action-view]:    https://github.com/rails/rails/blob/master/actionview/CHANGELOG.md
[action-mailer]:  https://github.com/rails/rails/blob/master/actionmailer/CHANGELOG.md
[action-cable]:   https://github.com/rails/rails/blob/master/actioncable/CHANGELOG.md
[active-record]:  https://github.com/rails/rails/blob/master/activerecord/CHANGELOG.md
[active-storage]: https://github.com/rails/rails/blob/master/activestorage/CHANGELOG.md
[active-model]:   https://github.com/rails/rails/blob/master/activemodel/CHANGELOG.md
[active-support]: https://github.com/rails/rails/blob/master/activesupport/CHANGELOG.md
[active-job]:     https://github.com/rails/rails/blob/master/activejob/CHANGELOG.md
[action-text]:    https://github.com/rails/rails/blob/master/actiontext/CHANGELOG.md
[action-mailbox]: https://github.com/rails/rails/blob/master/actionmailbox/CHANGELOG.md
[guides]:         https://github.com/rails/rails/blob/master/guides/CHANGELOG.md
