# JRE repository

## To serve up HTTP content

```bash
$ cd 'JRE_REPOSITORY_LOCATION'
$ python -m SimpleHTTPServer &
```

## Download required jres

Download the required JREs (x64) and place them in the oracle_jre/trusty/x86_64 folder. Update the oracle_jre/trusty/x86_64/index.yml with the updated jre.

## To Build new buildpack

In the example we are building version 4.5

### From bash:

```bash
$ git clone git@github.com:cloudfoundry/java-buildpack.git java-buildpack
$ cd java-buildpack
$ git fetch origin --tags
$ git checkout tags/v4.5 -b b4.5_oracle
```

### Build the openjdk offline buildpack

```bash
$ cd java-buildpack
$ bundle install
$ bundle exec rake clean package OFFLINE=true PINNED=true
```

Rename and copy the buildpack from build\java-buildpack-offline-v4.5.zip to a location on your machine.
Should be renamed: java-openjdk-buildpack-v4.5_20170911.zip

### Build the oracle offline buildpack

#### Updates to files

* Update the `config/components.yml`
** Comment out the `- "JavaBuildpack::Jre::OpenJdkJRE"` line
** Uncomment the `- "JavaBuildpack::Jre::OracleJRE"` line
* Overwrite the `config/oracle_jre.yml` with the `config/open_jdk_jre.yml`
** Update the `jre.repository_root` key to `"http://localhost:8000/oracle_jre/{platform}/{architecture}"`

#### Build

```bash
$ cd java-buildpack
$ bundle install
$ bundle exec rake clean package OFFLINE=true PINNED=true
```

Rename and copy the buildpack from build\java-buildpack-offline-v4.5.zip to a location on your machine.
Should be renamed: java-oracle-buildpack-v4.5_20170911.zip

### Pre-requisite for building

#### Ruby

Install Ruby by following instruction here: https://rvm.io/rvm/install. Use the version specified in the java-buildpack/.ruby-version file.

```bash
$ curl -sSL https://get.rvm.io | bash -s stable
$ rvm install ruby-2.2.7
$ rvm use 2.2.7
```

If not already done add in ~/.bash_profile:

`[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" # Load RVM into a shell session *as a function*`

#### Bundler

`gem install bundler`
