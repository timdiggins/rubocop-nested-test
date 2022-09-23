# Nested Rubocop test

The top level directory has a `.rubocop.yml` and excludes the nested-project.
It requires 'rspec-performance', and the Gemfile specifies it also.

When you run `rubocop` in the top directory it works as expected.

The nested-project directory represents a project nested within the top level project (see below for why it might occur).

The nested-project's `.rubocop.yml` does not require 'rspec-performance', and the Gemfile does not specify it.

if you run `rubocop` in the nested-project directory, it fails because rspec-performance gem can't be found. 

<details>
  <summary>output of `bundle exec rubocop --debug` from nested-project</summary>
  
```
For /rubocop-nested-test/nested-project: configuration from /rubocop-nested-test/nested-project/.rubocop.yml
Default configuration from ...../gems/rubocop-1.36.0/config/default.yml
AllCops/Exclude configuration from /rubocop-nested-test/.rubocop.yml
cannot load such file -- rubocop-performance
...../gems/rubocop-1.36.0/lib/rubocop/feature_loader.rb:46:in `rescue in rescue in load'
...../gems/rubocop-1.36.0/lib/rubocop/feature_loader.rb:39:in `rescue in load'
...../gems/rubocop-1.36.0/lib/rubocop/feature_loader.rb:32:in `load'
...../gems/rubocop-1.36.0/lib/rubocop/feature_loader.rb:21:in `load'
...../gems/rubocop-1.36.0/lib/rubocop/config_loader_resolver.rb:14:in `block (2 levels) in resolve_requires'
...../gems/rubocop-1.36.0/lib/rubocop/config_loader_resolver.rb:13:in `each'
...../gems/rubocop-1.36.0/lib/rubocop/config_loader_resolver.rb:13:in `block in resolve_requires'
...../gems/rubocop-1.36.0/lib/rubocop/config_loader_resolver.rb:12:in `tap'
...../gems/rubocop-1.36.0/lib/rubocop/config_loader_resolver.rb:12:in `resolve_requires'
...../gems/rubocop-1.36.0/lib/rubocop/config_loader.rb:46:in `load_file'
...../gems/rubocop-1.36.0/lib/rubocop/config_loader.rb:130:in `add_excludes_from_files'
...../gems/rubocop-1.36.0/lib/rubocop/config_loader.rb:110:in `configuration_from_file'
...../gems/rubocop-1.36.0/lib/rubocop/config_store.rb:68:in `for_dir'
...../gems/rubocop-1.36.0/lib/rubocop/config_store.rb:47:in `for_pwd'
...../gems/rubocop-1.36.0/lib/rubocop/cli.rb:99:in `parallel_by_default!'
...../gems/rubocop-1.36.0/lib/rubocop/cli.rb:46:in `run'
...../gems/rubocop-1.36.0/exe/rubocop:19:in `block in <top (required)>'
...../lib/ruby/2.7.0/benchmark.rb:308:in `realtime'
...../gems/rubocop-1.36.0/exe/rubocop:19:in `<top (required)>'
...../bin/rubocop:25:in `load'
...../bin/rubocop:25:in `<main>'
...../bin/ruby_executable_hooks:22:in `eval'
...../bin/ruby_executable_hooks:22:in `<main>'
```
</details>
  
This is very unexpected to me and I can't see why it is a desirable behaviour.

### Side note: Why would anyone have a project set up like this?

One reason I can think of is that you have a rails monorepo (hence the top level `.rubocop.yml` might specify rubocop-rails) and it has dependencies that can also be run as separate projects (hence may have no rails dependencies). And yes, there are arguments to have the rails project and the dependencies listed as sibling directories, or as separate git files, but either of these can make deployment of the rails project (the main project in the monorepo) harder.

