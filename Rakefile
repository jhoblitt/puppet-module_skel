require 'puppetlabs_spec_helper/rake_tasks'
require 'puppet-syntax/tasks/puppet-syntax'
require 'puppet-lint/tasks/puppet-lint'

begin
  require 'puppet_blacksmith/rake_tasks'
rescue LoadError
end

PuppetSyntax.exclude_paths = ['spec/fixtures/**/*']

PuppetLint::RakeTask.new :lint do |config|
  config.pattern          = 'manifests/**/*.pp'
  config.fail_on_warnings = true
end

task :travis_lint do
  sh "travis-lint"
end

task :default => [
  :validate,
  :lint,
  :spec,
]

task :acceptance do
  nodesets = Dir['spec/acceptance/nodesets/*.yml'].sort!.collect { |node|
    node.sub!('.yml', '')
    File.basename node
  }

  nodesets.each { |node|
    ENV['BEAKER_set'] =  node
    Rake::Task[:beaker].execute
  }
end
