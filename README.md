# CodeWorkout

## Setting up a Development Environment for CodeWorkout

The instructions below are aimed at macOS or Linux users. If you are on Windows, please jump to the section on a [Vagrant development environment](#using-a-virtual-machine-to-run-codeworkout).

### Installing Ruby

We recommend using Ruby version management software like [rbenv](https://github.com/rbenv/rbenv).
Installation instructions [here](https://github.com/rbenv/rbenv#installation).

Once you have `rbenv` installed, use it to install version 2.3.8:

```
rbenv install 2.3.8
```

Set the local version of Ruby (the one used in the `code-workout` project directory) to the one you just installed:

```
/path/to/code-workout $ rbenv local 2.3.8
```

Alternatively, you can install ruby on your machine using your operating system's package management software (e.g., `brew` on macOS, `apt` on Ubuntu, etc.).

### Installing Dependencies 

#### SQLite 

CodeWorkout uses SQLite in development. Install it using your package manager.

```bash
brew install sqlite3 # macOS
apt install sqlite3 # Ubuntu
```

#### Ruby gems
*Gems* are Ruby packages that help us do certain things (e.g., a gem that manages other gems!).

Issue the following command from your `code-workout` directory (the version number is important):

```
/path/to/code-workout $ gem install bundler -v 1.17.3
```

Then:

```
bundle install
```

### Populate the database
Once you have Ruby, SQLite, and dependencies installed, you need some data with which to populate the website.
Do the following:
```
rake db:populate
```

This will create a local database at `db/development.sqlite3` and populate it with seeded data (users, exercises, courses, etc.).

## Using a Virtual Machine to Run CodeWorkout

Vagrant is designed to run on multiple platforms, including Mac OS X, Microsoft Windows, Debian, Ubuntu, CentOS, RedHat and Fedora. In this document we describe how to configure and run CodeWorkout project virtual development environment through Vagrant.

### Installation Steps

1. Install [Vagrant](https://www.vagrantup.com/downloads.html)
2. Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
3. Clone this repository
4. `$ cd code-workout`
5. `$ vagrant up`
6. `$ vagrant ssh`
7. `$ cd /vagrant`
8. `$ ./runservers`
9. After the provisioning script is complete you can go to:

  * https://192.168.33.20:9292 for CodeWorkout server

### Shut Down The Virtual Machine

After you finish your work, you need to turn the virtual machine off.

1. Exit the virtual machine terminal by typing `exit`
2. `$ cd code-workout`
3. `$ vagrant halt`

### Re-run Development Servers

If you decided to shut down the virtual machine using `vagrant halt`, you have to re-run the servers again after you do `vagrant up`.

1. `$ cd code-workout`
2. `$ vagrant up`
3. `$ vagrant ssh`
4. `$ cd /vagrant`
5. `$ rails server`

### Reprovision The Virtual Machine

If anything went wrong or you want to reprovision your virtual machine for any reason, follow these steps.

1. `$ cd code-workout`
2. `$ git pull`
3. `$ vagrant destroy`
4. `$ vagrant up`

### Virtual Machine sudo password

sudo password is `vagrant` in case you need to execute any commands that require sudo.

### CodeWorkout Database Test Data

The initial database population is defined by lib/tasks/sample_data.rake.
It uses the factories defined in spec/factories/* to generate entities.
If you add new modael classes and want to generate test data in the
database, please add to the sample_data.rake file so that this population
will happen automatically for everyone.  The sample_data.rake contains
only "sample/try out" data for use during development, and it won't
appear on the production server.  Initial database contents provided
for all new installs, including the production server, is described
in db/seeds.rb instead.

  - The initial database includes the following accounts:
    - admin@codeworkout.org (pass: adminadmin), has admin-level access
    - example-1@railstutorial.org (pass: hokiehokie), has instructor access
    - example-*@railstutorial.org (pass: hokiehokie) 50 students

    It also includes the following other objects:
    - six terms (spring, summer I, summer II, fall, and winter 2016),
    - one organization (VT)
    - one course (CS 1114)
    - two offerings of 1114 (one each semester)
      - one course offering is set up with the admin and instructor
        as instructors, and all other sample accounts as students

  - To reset the database to the initial state do the following:
    - `$ cd code-workout`
    - `$ vagrant ssh`
    - `$ cd /vagrant`
    - `$ rake db:populate`
    - `$ rails server`

### Keep code-workout repository up to date

During development of code-workout, other developers might add new gems to the project or add new migrations etc. To keep your local version up to date with the latest version do the following:

- Open a new terminal
- `$ cd code-workout`
- `$ git pull`
- `$ vagrant reload`
- `$ vagrant ssh`
- `$ cd /vagrant`
- `$ sudo bundle install`
- `$ rake db:populate` **Note:** This step will place the database in a simple starter state.
- `$ rails server`


### Upload and Test Programming Exercises

- Log in using your admin account admin@codeworkout.org (pass: adminadmin)
- Click "Upload Exercises" in the navigation bar, then click "Choose File". Choose your exercise file and click "Submit File".
- Click "Create Workouts" in the navigation bar to create new workouts.
- Go to the admin area by clicking on the little wrench icon to the left of the admin email address in the top menu bar. Click the "University-oriented" menu and select "Workout offerings". Click on "New Workout Offering". Create your new workout offering by choosing the course offering and the workout (the one you have just created in the previous step). Then select appropriate opening, soft deadline, and hard deadline dates for the workout offering and hit "Create Workout Offering" button to save it.
- Log in using student account example-2@railstutorial.org (pass: hokiehokie). Then navigate to the course offering you have chosen in the previous step, you should see the new workout in the list of workouts for this course offering.

### Connect to CodeWorkout Database

During development it is convenient to connect to CodeWorkout database from your host machine using [MySQL Workbench](https://www.mysql.com/products/workbench/). Once you installed MySQL Workbench create a new connection to CodeWorkout Database in the Vagrant machine using the following setup:

- Connection Name: CodeWorkout
- Connection Method: Standard TCP/IP over SSH
- SSH Hostname: 192.168.33.20
- SSH Username: vagrant
- SSH Password: vagrant
- MySQL Hostname: 127.0.0.1
- MySQL Server Port: 3306
- Username: codeworkout
- Password: codeworkout

