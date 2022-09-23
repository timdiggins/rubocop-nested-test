# Nested Rubocop test

The top level directory has a `.rubocop.yml` and excludes the nested-project.
It requires 'rspec-performance', and the Gemfile specifies it also.

When you run `rubocop` in the top directory it works as expected.

The nested-project directory represents a project nested within the top level project (see below for why it might occur).

The nested-project's `.rubocop.yml` does not require 'rspec-performance', and the Gemfile does not specify it.

if you run `rubocop` in the nested-project directory, it false because rspec-performance gem can't be found. 

This is very unexpected to me and I can't see why it is a desirable behaviour.

### Side note: Why would anyone have a project set up like this?

One reason I can think of is that you have a rails monorepo (hence the top level `.rubocop.yml` might specify rubocop-rails) and it has dependencies that can also be run as separate projects (hence may have no rails dependencies). And yes, there are arguments to have the rails project and the dependencies listed as sibling directories, or as separate git files, but either of these can make deployment of the rails project (the main project in the monorepo) harder.

