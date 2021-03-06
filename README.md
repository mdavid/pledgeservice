# Lessig2016

## tl;dr

* **Applicable skills** TBD
* **Slack** [#gh-pledgeservice](https://teamlessigtech.slack.com/messages/gh-pledgeservice/)
* **Project lead** [@sean](https://teamlessigtech.slack.com/team/sean)

## Design sketch

The majority of the site is just basic static content, as it should
be. That markup lives in "markup/" in the form of jade files. If you
don't know what those are, they're very simple, and remove literally
50% of HTML's boilerplate so they're worth it. Read the 3 minute
tutorial.

Stylesheets are in "stylesheets/" as sass files. See last paragraph
for why that's good.

Ideally there will be little enough JS that no framework will be necessary (fingers crossed).

The backend will be very simple with two endpoints

1. Pledge. This has to be done in coordination with stripe so that stripe stores the credit card info, and we only store an opaque token and the pledge amount. This will write to what'll be probably the only table in the datastore.
2. GetTotal: Simple sum over pledges. Store it in memcache, expire every few minutes. Boom.

## Dependencies

* Python App Engine SDK
* NPM
* sass (with ruby installed, `gem install sass`)

## Hacking

After checking out the code, run `npm install`. To start the server, run `npm start` and go to
[http://localhost:8080](http://localhost:8080). That's it!


### Running Tests

After running `npm install` and `grunt local`, copy `build/config.json` over to `backend/` and run
`python testrunner.py` in the project's root directory.

(Test have not been run on this codebase for a while. . . )


## Deploying

We have 4 deployment environments available, all of which can be set up with grunt (installed by npm).
* **local**: For normal development, with code updates on every reload. Run `npm start` or equivalently `node_modules/.bin/grunt local`.
  * Note that in local mode, you can't actually send a transaction to stripe due to an SSL bug in dev_appserver. But
    you can get right up to that point before it fails, which is generally good enough.
* **dev**: This is an independant instance of the app running at https://pledge-test.lessig2016.us/pledge.. We can do
  whatever we want here because the data's all fake. It also uses Stripe's test keys, so feel free to submit test
  transactions with credit card 4242 4242 4242 4242. To deploy, run `./node_modules/.bin/grunt dev`, and then
  `appcfg.py --oauth2 update build/`.
  When testing, it works best to hit this URL: https://pledge-test.lessig2016.us/pledge.

* **lessig**: The real McCoy. Don't break it. To deploy, run `./node_modules/.bin/grunt lessig`, and then
  `appcfg.py --oauth2 update build/`.

## Troubleshooting

If `npm start` generates an out of files error and prints a lot of messages like this:

    Warning: EMFILE: Too many opened files.

You probably have an orphan app engine process running. Try looking for and
killing any child processes. Worst case, you may need to increase your inotify
maximums; you can do that on a Linux system with this command:

    echo 1024 > /proc/sys/fs/inotify/max_user_instances

## Code of Conduct

Lessig2016 is committed to fostering an open and inclusive community where engaged, dedicated volunteers can build the strategy and tools necessary to fix our country's democracy. All members of the community are expected to behave with civility, speak honestly and treat one another respectfully.

This project adheres to the [Open Code of Conduct](http://todogroup.org/opencodeofconduct/#Lessig2016/conduct@lessigforpresident.com). 
By participating, you are expected to honor this code.

This reference as well as the included copy of the [Code of Conduct](https://github.com/Lessig2016/pledgeservice/blob/master/CONDUCT.md)
shall be included in all forks and distributions of this repository.

## Legal

The Lessig campaign is not responsible for the content posted to this repository, or for actions taken or liabilities incurred by one or more group members. 

You may not post content to the repository that you do not own, and by posting content you consent to its use by others, including the campaign. 

You are responsible for your compliance with federal campaign finance laws. You may not, for example, volunteer for the campaign if you are being paid by someone else to do so, or volunteer while at work unless it is limited to a few hours per month and your volunteering doesn’t create additional costs for your employer.

The campaign may remove any offensive, abusive, attacking, or discriminatory content, as well as any content that may otherwise violate our [Terms of Service](https://lessig2016.us/terms-of-service/). 

## Copyright and License

Copyright 2015 Lessig2016. This 
project is released under [GNU Affero General Public License, version 3](https://github.com/Lessig2016/pledgeservice/blob/master/LICENSE).

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
