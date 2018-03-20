# Minesweeper

Minesweeper created with laravel and vuejs

## Installation

### Prerequisites

* To run this project, you must have PHP 7 installed.
* Composer is used for dependency management in PHP. https://getcomposer.org/doc/00-intro.md
* You requiere node and npm for front end dependencies and builds. Development versions are 8.9.1 and 5.5.1 respectively.
* You should setup a host on your web server for your local domain. In development mode you could also configure Laravel Homestead or Valet. 
* This project uses Laravel Mix ^2.0 to simplify webpack setup. https://laravel.com/docs/5.6/mix

### Step 1

Begin by cloning this repository to your machine, and installing all Composer & NPM dependencies.

```bash
git clone git@github.com:lgalaz/minesweeper.git
cd vuejs-minesweeper && composer install && npm install
cp .env.example 
php artisan key:generate
npm run dev
```

* Note: run 'npm run' to see scripts available in npm. 

### Step 2

Next, boot up a server and visit your forum. 
If using a tool like Laravel Valet, the URL will default to `http://vuejs-minesweeper.test`. 


## Demo

http://104.236.111.126/
