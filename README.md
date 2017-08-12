# OpenBenches

![A bench in a park, birds fly up above. In the background is a tree.](https://openbenches.org/mstile-70x70.png) https://OpenBenches.org/ - an open data repository for memorial benches.

# BETA

OMG! This code is still a bit horrible. Looking at it will make you a bit sad.
This is a *slightly grubby* beta - thrown together to test whether things work, but now we are sharing the blame with all the folks who have contributed so far.

## Contributing

All contributions are welcome.  Before making a pull request, please:

1. Raise a new issue describing the problem and how you intend to fix it.
2. Submit a Pull Request referencing the Issue.

## Running Locally

This is a simple PHP and MySQL website. No need for node, complicated deploys, or spinning up containerised virtual machines in the cloud.

### Requirements

* PHP 7 or greater.
* MySQL 5.5 or greater with innodb.
* ImageMagick 6.9.4-10 or greater.

### External APIs

You will need to sign up to some external API providers:

* Reverse Geocoding requires an [OpenCage API key](https://geocoder.opencagedata.com/)
* Flickr Import requires a [Flickr API key](https://www.flickr.com/services/api/)
* Tweeting requires a [Twitter Developer API key](https://apps.twitter.com/)

Add them to `config.php.example` - rename that to `config.php`

### Database Structure

In the `/database/` folder you'll find a sample database.  All text fields are `utf8mb4_unicode_ci` because we live in the future now.

Hopefully, the tables are self explanatory:

#### Benches

* `benchID`
* `latitude`
* `longitude`
* `address` text representation generated by reverse geocoding. For example "10 Downing Street, London SW1A 2AA, United Kingdom"
* `inscription`
* `description` placeholder. Might be used for comments about the bench.
* `present` placeholder. If a bench has been physically removed, this can be set to false.
* `published`
* `added` datetime
* `userID` foreign key

#### Users

Originally we were going to force people to sign in with Twitter / Facebook / GitHub. But that discourages use - so users are now pseudo-anonymous. Hence this weird structure!

* `userID`
* `provider` could be Twitter, GitHub, etc.
* `providerID` user's name on the provider's service.  Anonymous users stores their IP address.
* `name` their display name. Anonymous users stores the time they added a bench.

#### Media

We store the original image - smaller images are rendered dynamically.

Media storage can be complicated. Storing thousands of images in a single directory can cause problems on some systems. To get around this, we calculate the [SHA1 hash](https://en.wikipedia.org/wiki/SHA-1) of each image. The image is stored in a subdirectory based on the hash.  For example, if the hash is `1A2B3C`, the file will be stored in `/photos/1/A/1A2B3C.jpg`

* `mediaID`
* `benchID`
* `userID`
* `sha1` A hash of the file.
* `importURL` If the image was imported from an external source - like Flickr.
* `licence` The default is `CC BY-SA 4.0`, imported images may be different.
* `media_type` We allow different types of photo - in the future, we might have other types of media.

#### Media Types

At the moment, we only accept photos - of the inscription, the bench, the view from the bench, a panorama, and a VR photosphere.

* `shortName` Internal ID.
* `longName` Displayed to the user.
* `displayOrder` When rendering a form in HTML, this determines the order they are presented in.

#### Licences

* `shortName` Internal ID.
* `longName` Displayed to the user.
* `url` For more information.

## Open Source Licenses

Everything we do builds on someone else's hard work.

* OpenBenches data are made available under the [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/).
* The code powering the website is [MIT](https://opensource.org/licenses/MIT).
* All photos uploaded are [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).
* [Benches from Bath](https://github.com/BathHacked/banes-geographic-data/blob/master/banes_park_seats_and_benches.geojson) are [OGL](http://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/) and Powered by [Bath: Hacked](https://www.bathhacked.org/).
* Logo template by [Creative Mania](https://thenounproject.com/term/park/923893/) [CC BY](http://creativecommons.org/licenses/by/3.0/us/).
* Twitter integration by [CodeBird](https://github.com/jublonet/codebird-php) [GPL v3](https://www.gnu.org/licenses/gpl-3.0.en.html).
* Maps by [Leaflet](https://github.com/Leaflet/Leaflet) [BSD 2-clause "Simplified" License](https://opensource.org/licenses/BSD-2-Clause).
* Map tiles by [MapBox](https://www.mapbox.com/).
* Social Media logos by [Agata](https://www.iconfinder.com/iconsets/social-hand-drawn-icons).
* GPS logo by [Chinnaking](https://thenounproject.com/term/gps/1050710/) [CC BY](http://creativecommons.org/licenses/by/3.0/us/).
* Panoramic Visualiser by [Pannellum](https://pannellum.org/) [MIT](https://opensource.org/licenses/MIT).
