# JBot

A sweet IRC bot with a **web interface** for [Jupiter Broadcasting](http://www.jupiterbroadcasting.com/).
Built on [cinch](https://github.com/cinchrb/cinch) and [sinatra](http://www.sinatrarb.com/). It is a fork/evolution of
Showbot, built for 5by5.

## IRC Commands

* !next - When's the next live show?
* !schedule - What shows are being recorded live in the next seven days?
* !suggest - Be heard. Suggest a title for the live show.
* !link - Know the link for that? Suggest it and make the show better.
* !current - What's playing on jblive.tv? I've got you covered.
* !last_status - The last tweet by @ChrisLAS, delivered to you in IRC. Sweet.
* !about - Was JBot coded or did it spontaneously come into existence?
* !help - Uh, this.

## Setup and Customization

### Prerequisites

 * [RVM with Ruby 1.9.2 or Greater](https://rvm.io/)
 * [Bundler](http://gembundler.com/)

### Setup

These commands will get you setup to run Showbot.

 * `git clone https://github.com/rikai/Showbot.git`
 * `cd Showbot`
 * `bundle`
 * `foreman run rake db:migrate`

Finally you need to setup your `.env` file in the root of the project. At the
bare minimum you'll need the following:

```
# Bot lib folder
RUBYLIB=./lib

# Set this if you want to use a language other than english.
# You will also need to create a corresponding .yml file in the locales folder.
SHOWBOT_LOCALE=en

# Foreman stuff
## Production port
PORT=80
## Development port
DEVELOPMENT_PORT=5000

# Point this to the url of data.json, if you have one
SHOWBOT_DATABASE_URL=your_info_here

# For backup.rb
BOT_DATABASE_NAME=your_info_here
BOT_DATABASE_USER=your_info_here
BOT_DATABASE_PASSWORD=your_info_here
BOT_DATABASE_HOST=your_info_here
BOT_DATABASE_PORT=your_info_here
BOT_DATABASE_OPTS=your_info_here
S3_ACCESS_KEY_ID=your_info_here
S3_SECRET_ACCESS_KEY=your_info_here
S3_REIGON=your_info_here
S3_BUCKET=your_info_here
S3_PATH=your_info_here
S3_KEEP=your_info_here

# Point this to the url of data.json, if you have one
DATA_JSON_URL=your_info_here
```

### Configuring IRC

 * Customize [`cinchize.yml`][cinchize] for your IRC channel.
 * Update [`fix_name`][fix_name] to match your bot's name.

[cinchize]: https://github.com/rikai/Showbot/blob/master/cinchize.yml
[fix_name]: https://github.com/rikai/Showbot/blob/master/lib/cinch/plugins/showbot_admin.rb#L54

### data.json

`data.json` is a file served specifically by 5by5.tv that indicates which
show is live. It is currently integrated with title suggestions so that the
currently playing show is fetched and saved in the database when a title is
suggested.

If you want to remove the functionality provided by `data.json`, you will need
to start by removing the [before create hook][hook] in `suggestion.rb`. To avoid
the "Show Not Listed" message you'll want to remove the suggestion set break in
[`_table_set.haml`][table_set] and [`_bubble_set.haml`][bubble_set].

[hook]: https://github.com/rikai/Showbot/blob/master/lib/models/suggestion.rb#L45
[table_set]: https://github.com/rikai/Showbot/blob/master/views/suggestion/_table_set.haml#L4
[bubble_set]: https://github.com/rikai/Showbot/blob/master/views/suggestion/_bubble_set.haml#L3

### Modifying the CSS

Modifying [`showbot.scss`][showbot_scss] requires that you start the `rake sass:watch`
command. While this command is running, `public/showbot.css` will be
overwritten with any changes that are made in `showbot.scss`. This annoying
setup is necessary due to [Bourbon](https://github.com/thoughtbot/bourbon) not
working well outside of the Rails asset pipeline.

[showbot_scss]: https://github.com/rikai/Showbot/blob/master/sass/showbot.scss

### Data.json
<!-- FIXME: CORRECT THIS TO MATCH THE CURRENT IMPLEMENTATION -->
5by5.tv uses a JSON API to provide details about which show is live. When a
title is suggested, this JSON API is queried and the show that the title was
suggested during is attached to the suggestion.

The format of 5by5.tv's JSON is as follows:

```
  {
    'live': true,
    'broadcast': {
      'slug': 'show_slug_here'
    }
  }
```

The `'show_slug_here'` is the value read and attached to the title suggestion.

### Launching Showbot

**Website and the IRC Bot**

```
$ bundle exec foreman start -f Procfile.local
```

**Just the Website**

```
$ bundle exec foreman start web -f Procfile.local
```

**Just the IRC Bot**

```
$ bundle exec foreman start irc -f Procfile.local
```

## Special Thanks

 * Special thanks to Rikai for reverse-engineering the setup steps for someone
   setting up Showbot from scratch.
 * To [gouwens](https://github.com/gouwens) for implementing the clustered
   view.

## JBot on the Internets

[The Creation of Showbot](http://pileofturtles.com/2011/07/showbot/)

