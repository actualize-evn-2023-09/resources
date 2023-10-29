# Guide: Rails migrations

## To create a database:

1. In the terminal, run: `rails db:create` (alt: `rake db:create`)

   - _Note: you only need to do this once per app_

## To create a new table in the database:

1. In the terminal, run:

   ```
   rails generate model Product name:string description:text 'price:decimal{5,2}'
   ```

2. DOUBLE CHECK THE GENERATED MIGRATION FILE CODE (in the db/migrate folder)

   - _If there are typos, you can fix them now before doing the next step_

3. In the terminal, run: `rails db:migrate` (alt: `rake db:migrate`)

   - _Note: A description of rails database column types can be found here:_

     http://stackoverflow.com/questions/11889048/is-there-documentation-for-the-rails-column-types

## To make changes to existing database tables:

1. In the terminal, run: `rails generate migration AddImageToProducts`

   - _In this example, you will be adding an image column to the products table_

2. Add code to the new migration file generated in the db/migrate folder

   - Add a column: `add_column :products, :image, :string`
     - _In this example, image is the new column, string is its data type_
   - Rename a column: `rename_column :products, :image, :photo`
     - _In this example, image is the old name, photo is the new name_
   - Remove a column: `remove_column :products, :photo, :string`
     - _In this example, photo is the column to remove, string is its data type_
   - Change a data type: `change_column :products, :quantity, :integer`
     - _In this example, quantity will become an integer_
   - Using decimals (to add a column): `add_column :products, :price, :decimal, precision: 5, scale: 2`

     - _Precision is the total number of digits, scale is the number of digits after the decimal_

     - _In this example, 103.42 is a valid price, 3432.21 isn’t (total digits is greater than precision)_
     - _NOTE: postgres has issues changing strings to decimals, use these 2 lines in a migration:_

       ```
       change_column :products, :price, "numeric USING CAST(price AS numeric)"
       change_column :products, :price, :decimal, precision: 9, scale: 2
       ```

   - All available commands here:
     http://api.rubyonrails.org/classes/ActiveRecord/Migration.html

3. DOUBLE CHECK YOUR MIGRATION FILE CODE (in the db/migrate folder)

   - _If there are typos, you can fix them now before doing the next step_

4. In the terminal, run: `rails db:migrate`

- Shortcuts for steps 1 and 2:
  - To add a column to a table:
    `rails generate migration AddImageToProducts image:string`
  - To remove a column from a table:
    `rails generate migration RemovePhotoFromProducts photo:string`
  - No other shortcuts exist - if you need to rename, change, etc., you’ll need to generate an empty migration file and write the code in as described above

## To generate sample data for database:

1. Write code in the db/seeds.rb file (instead of through the rails console)

2. In the terminal, run: `rails db:seed`
   - Note: If you already have sample data created from rails console or your application, you can use this gem to generate a seed file so others can reproduce it: https://github.com/rroblak/seed_dump
