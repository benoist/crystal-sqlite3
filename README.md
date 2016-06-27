# crystal-sqlite3 [![Build Status](https://travis-ci.org/manastech/crystal-sqlite3.svg?branch=master)](https://travis-ci.org/manastech/crystal-sqlite3)

SQLite3 bindings for [Crystal](http://crystal-lang.org/).

**This is a work in progress.**

[Documentation](http://manastech.github.io/crystal-sqlite3/)

### shard.yml

```yml
dependencies:
  sqlite3:
    github: manastech/crystal-sqlite3
```

### Usage

```crystal
require "db"
require "sqlite3"

DB.open "sqlite3://./data.db" do |db|
  db.exec "create table contacts (name string, age integer)"
  db.exec "insert into contacts values (?, ?)", "John Doe", 30

  args = [] of DB::Any
  args << "Sarah"
  args << 33
  db.exec "insert into contacts values (?, ?)", args

  puts "max age:"
  puts db.scalar "select max(age) from contacts" # => 33

  puts "contacts:"
  db.query "select name, age from contacts order by age desc" do |rs|
    puts "#{rs.column_name(0)} (#{rs.column_name(1)})"
    # => name (age)
    rs.each do
      puts "#{rs.read(String)} (#{rs.read(Int32)})"
      # => Sarah (33)
      # => John Doe (30)
    end
  end
end
```
