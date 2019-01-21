# Example edx microsite
This tutorial extends the [Microsites Theming overview] (https://github.com/edx/edx-platform/wiki/Microsites-Theming) by providing instructions how to configure and add an example edx-microsite `ntua.localhost`.

+ Edit the */etc/hosts*  file on your host machine via `sudo nano /etc/hosts` by adding the following line
```
 127.0.0.1			ntua.localhost
```
+ If you are developing locally, stop all other running vagrant images of Open edX
+ Copy the *edx-microsite/* directory from this repo inside a parent directory to *edx-platform/*
 + This should leave your folder structure looking like this:
  ```
  - /
    - edx-platform/
    - edx-microsite/
      - ntua/
        - css/
        - images/
        - templates/
    ...
  ```
+ If you are developing locally, you need to add your newly created *edx-microsite/* folder to vagrant's [synced folders](https://docs.vagrantup.com/v2/synced-folders/) by modifying the `Vagranfile`. An example of a configured *Vagrantfile* is provided in this repo.
+ Edit the *lms.env.json* file via `sudo nano /edx/app/edxapp/lms.env.json`  by adding the following lines:
  ```
  ...
  "FEATURES": {
   ...
   "USE_MICROSITES": true
  },
  ...
  "MICROSITE_CONFIGURATION": {
    "ntua": {
        "domain_prefix": "ntua",
        "university": "NTUA",
        "platform_name": "NetMode Education Online Programs",
        "logo_image_url":  "ntua/images/header-logo.png",
        "ENABLE_MKTG_SITE":  false,
        "SITE_NAME": "ntua.localhost",
        "course_org_filter": "NetMode",
        "course_about_show_social_links": false,
        "css_overrides_file":  "ntua/css/ntua.css",
        "show_partners":  false,
        "show_homepage_promo_video": false,
        "homepage_promo_video_youtube_id": "afyACWk95YU",
        "course_index_overlay_text": "Explore NetMode courses",
        "homepage_overlay_html":  "<h1>Welcome to NETMODE Laboratory</h1><h2>Online Courses powered by Open edX<sup>®</sup> Platform</h2>",
        "favicon_path": "ntua/images/ntua-logo.png",
        "ENABLE_THIRD_PARTY_AUTH": false,
        "ALLOW_AUTOMATED_SIGNUPS": true,
        "ALWAYS_REDIRECT_HOMEPAGE_TO_DASHBOARD_FOR_AUTHENTICATED_USER": false,
        "course_email_from_addr": "netmode_edx@ntua.gr",
        "SESSION_COOKIE_DOMAIN": "ntua.localhost"
      }
  },
  "MICROSITE_ROOT_DIR": "/edx/app/edxapp/edx-microsite",
  ...
  ```
*An example of an entire lms.env.json file is provided in the repo.*

+ If you are developing locally:
 + restart the vagrant VM for the Vagrantfile changes to take effect:
 + ssh back into the Vagrant `vagrant ssh `
 + start LMS: `sudo su edxapp; paver devstack lms`
 + if things are configured correctly, you should see something like this in the output console:
  ```
  [lms.startup] startup.py:127 ­ Loading microsite
  /edx/app/edxapp/edx­microsite/ntua
  ```
+ You should now be able to bring up a web browser and go to http://ntua.localhost:8000
+ In order for a course to appear in the site catalog, you will need to create it in Studio with `organization` set to ‘NetMode’.
