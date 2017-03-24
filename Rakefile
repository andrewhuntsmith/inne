require 'active_record'
require 'yaml'

namespace :db do
  task :environment do
    DATABASE_ENV = ENV['DATABASE_ENV'] || 'development'
    MIGRATIONS_DIR = ENV['MIGRATIONS_DIR'] || 'db/migrate'
  end

  task :configuration => :environment do
    @config = YAML.load_file('db/config.yml')[DATABASE_ENV]
  end

  task :configure_connection => :configuration do
    ActiveRecord::Base.establish_connection(@config)
  end

  task :create => :configure_connection do
    ActiveRecord::Base.establish_connection(@config)
  end

  task :drop => :configure_connection do
    ActiveRecord::Base.connection.drop_database(@config['database'])
  end

  task :migrate => :configure_connection do
    ActiveRecord::Migrator.migrate(MIGRATIONS_DIR, ENV['VERSION'] ? ENV['VERSION'].to_i : nil)
  end

  task :rollback => :configure_connection do
    ActiveRecord::Migrator.rollback(MIGRATIONS_DIR, (ENV['STEP'] || 1).to_i)
  end

  task :seed => :configure_connection do
    require_relative 'Models.rb'
    require_relative 'db/seeds.rb'
  end
end
