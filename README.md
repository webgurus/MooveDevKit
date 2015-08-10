
# Moove Development Kit

* Based on Sage: [https://github.com/roots/sage](https://github.com/roots/sage)
* Using WPEngine Mercury on Vagrant [http://wpengine.com/mercury/how-to-start/](http://wpengine.com/mercury/how-to-start/)
* Using GitHub and CodeShip for deployment [http://codeship.com](http://codeship.com)
* Home Page: [http://mooveagency.com/](http://mooveagency.com/)

## Prerequisites ##
In order to use MDK effectively, you'll need to have a few tools installed on your computer. You should:

1. Install [Git](http://git-scm.org).
 * Windows users: Be sure to add the Git executables to your path (See, e.g. [this guide](https://eamann.com/tech/vagrant-windows/), under "Prerequisites")
2. Install virtual machine software [VirtualBox](http://virtualbox.org).
3. Install [Vagrant](http://vagrantup.com)
4. **Highly recommended:** Install the [Vagrant Hostsupdater plugin](https://github.com/cogitatio/vagrant-hostsupdater)
 * Short version: `vagrant plugin install vagrant-hostsupdater`

### Check yoru installations

Now, let’s go ahead and check out the tools we have installed. To start with Git, type `git --version` in the command line. For Vagrant, type `vagrant -v`, and then for VirtualBox, use command `vBoxManage -v`.

## WPEngine Mercury Installation ##
1. Create a directory where your Vagrant and your sites will be stored. e.g. 'moovevagrant'.
1. `git clone --recursive https://github.com/wpengine/hgv.git moovevagrant` to clone the latest version of WPE Mercury in the 'moovevagrant' directory.
2. Change into the directory:  `cd moovevagrant`.
3. Run `vagrant up`.
4. Access `http://hvg.dev/` to read more about what tools and URLs you can work with.

Please read the [full how-to on how to set up Mercury](http://wpengine.com/mercury/how-to-start/) to fully understand what is happening when you execute the above commands.


## Developing using Vagrant/Mercury: ##

### 1. Setting up multiple development sites ###

Vagrant is basically a virtual server fully set up and it allows you to have multiple "dev" installations running at the same time. The default installation is called **hgv.dev** and it is setup automatically when you run `vagrant up` for the first time. Vagrant creates a shared folder called **hgv_data** in the directory where you installed Mercury and here you can find all the files related to your development sites and the default site as well. For this documentation we will use "UKIBC" as the example project.

To set up multiple sites on the same host you have to do the following:

1. Copy `/moovevagrant/provisioning/default-install.yml`
2. Create a directory called `config/` in `/moovevagrant/hgv_data/`
3. Paste your copied file (`default-install.yml`) to the config directory and rename it to whatever your new development domain will be (e.g. ukibc.dev)
4. Edit the `ukibc.yml` file and modify it's content to reflect your new development domain. Your file should look like this:
```
 wp:
  enviro: ukibc
  hhvm_domains:
    - ukibc.dev
    - ukibc.test
  php_domains:
    - php.ukibc.dev
    - php.ukibc.test
```
To allow Vagrant to install the new domain and provision the server accordingly, you need to reload the server by running `vagrant reload`  and provision it after it finished reloading `vagrant provision`.

Your site is up and running locally, so here's how you can access everything you need:

* You can access the WordPress files of the new site in your `/moovevagrant/hgv_data/sites/<DEV_SITE_NAME>/` directory.
* In the above directory you will find a file called `local-config.php`. This file contains the connection information for the development site's database. It usually is generated like this (where 'ukibc' changes to the actual dev site you will be setting up):
```
/** The name of the database for WordPress */
define('DB_NAME', 'wpe_ukibc');

/** MySQL database username */
define('DB_USER', 'wpe_ukibc');

/** MySQL database password */
define('DB_PASSWORD', 'wordpress');
```
* You can see your development site at: [http://ukibc.dev](http://ukibc.dev)
* You can access phpMyAdmin at: [http://admin.hgv.dev/phpmyadmin](http://admin.hgv.dev/phpmyadmin)
* You can access the new site's admin area in the usual way, by accessing [http://ukibc.dev/wp-admin/](http://ukibc.dev/wp-admin/)
* The default WordPress admin user is `wordpress` and the password is `wordpress`.

### 2. Accessing logs and debugging related tools ###
Because Mercury has automatically set up the default domain hgv.dev, it installs every tool under this domain.

Please refer to the Getting Started section of the HGV install by accessing: [http://hgv.dev/#mercury-vagrant-hgv-admin-tools](http://hgv.dev/#mercury-vagrant-hgv-admin-tools) to read about debugging and admin tools.

## HTML development
Start by [downloading MooveDevKit](https://github.com/MooveAgency/MooveDevKit/zipball/master) and then unzip and copy the contents of the downloaded archive to the directory where your WordPress files reside and merge/replace when asked. Following the above instructions the directory where you need to paste your files would be `/moovevagrant/hgv_data/sites/ukibc/`.

Then you should do the following modifications:

1. Go to the wp-content folder - `cd moovevagrant/hgv_data/sites/ukibc/wp-content/themes/`
2. Rename the `sage-moove/` directory to the project's name, for example `ukibc-theme`.
3. Change the theme name in the *style.css* file.

###Version control

To work on a new project you must set up a new repository for it on GitHub and use Git on a regular basis. This is a **must** in our development process because otherwise this whole process is worthless.

To set up a new GitHub repository, you need to be added to the MooveAgency organisation. If you are still not part of this organisation on GitHub, please e-mail [Lukas](mailto:lukas@mooveagency.com), [Marton](marton@mooveagency.com) or [Ilona](mailto:ilona@mooveagency.com) with your GitHub username and e-mail address.

You need to create a Repository under the the MooveAgency Organisation page [https://github.com/MooveAgency](https://github.com/MooveAgency) by clicking the New Repository button.

The Repository should be named after your project, e.g. "ukibc-theme". Once you've create the repository you will need to add the repository as the remote to your project. To achieve that you will have to do the following:

1. Go to your WordPress folder running on Vagrant (where the wp-config.php resides for this project), in this case */moovevagrant/hgv_data/sites/ukibc/*.
2. Initialise an empty Git repository in this folder.
3. Add a remote repository to your local one.

The commands for the above listed steps should be these:

`git init`

then add your remote

`git remote add origin https://github.com/MooveAgency/ukibc-theme.git`

then you have to add all your files to create the initial commit like this:

`git add .`

 where the dot actually adds all the files. You can check the status of the repository or what files should be added by issuing `git status`.

Next you have to commit for the first time by adding a message to your commit:

`git commit -m 'Initial commit'`

and then you have to push your first commit:

`git push origin master`

After the push was successful you need to create the develop branch by issuing the following command:

`git branch develop`

then checkout the newly created branch to start working on it:

`git checkout develop`

After a couple of commits when you want to push to your remote repository, you will have to slightly change the previous push command to:

`git push origin develop`


####Commit

There are a couple of guidelines on how and why to commit:

1. You should commit every 30 minutes, even if you haven't finished your feature.
2. You should commit when you finished a feature
3. When adding a commit message, you should be really descriptive but in the same time you have to be short, so you will definitely know what commit was related to what exactly.

Example commit messages would be something like this:

1. You are just finishing a feature called "Newsletter Widget", then you would commit like this:

`git commit -m 'Finished feature: Newsletter Widget'`

####Cloning an already exisiting repository in your local environment

There are two situations when you need to clone an exisiting repository to your local environment:

1. When you start working on a project that is already under development.
2. When you want to modify just a small thing and you don't need to pull the repo into an existing WordPress install.

Go into your local development folder...if the folder is empty, you can use:

`git clone git@github.com:whatever .`

else

```
git init
git remote add origin PATH/TO/REPO
git fetch
git checkout -t origin/develop
```
The above procedure should be used when you try to pull an already existing theme into your local Vagrant/Mercury setup. This means that the folder is not empty (it contains the WordPress install already).

###Development

[Install Gulp](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md), and then install the dependencies for Sage contained in `package.json` by running the following **from the theme directory**:

```
npm install
bower install
```
After everything was installed correctly, you can type `gulp` to build the assets for the first time. This is important, because if you don't build for the first time, you won't be able to see the CSS and JavaScript files when previewing your HTML files.

After the assets were built, you can use `gulp watch` to watch for changes in the SASS and JS files and let gulp automatically re-build when you save your changes.

#### Directory Structure ###
The structure related to the HTML development include the following directories:

* **/theme_name** - in this case it is /ukibc-theme
	*  **/assets** - the editable SASS and JS files can be found here
	* **/dist** - the files built by gulp will be copied here
	* **/html** - all the html files should reside here
	* **/bower_components** - all the vendor packages will be installed here via bower

When developing the HTML files your working directory should always be the  `html/` folder found in the wordpress theme folder. In the context of this documentation it would be: `/moovevagrant/hgv_data/sites/ukibc/wp-content/themes/ukibc-theme/html/`, and you have to refer to the `../dist/` directory when you are including an asset, like so:

```
<link href="../dist/styles/main.css" rel="stylesheet">
<script src="../dist/scripts/modernizr.js"></script>
```

**Note:** *Please do not touch other files that are usually ending in **.php** and other folders like {lib|templates|inc}.*

## Theme Development
After you've installed Gulp and ran `npm install` from the them folder, use `gulp watch` to watch for updates to your SASS and JS files and Gulp will automatically re-build as you write your code.


## Features

* Organized file and template structure
* HTML5 Boilerplate's markup
* Bootstrap
* Gulp build script