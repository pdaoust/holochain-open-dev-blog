---
layout: post
title: How To Ship Your hApp
tags: [ship, holochain, release]
author: Connoropolous 
---

It has been a long journey that I've been on with Holochain (since 2017, among the first developers to build apps with the Go prototype of Holochain), and one of the questions I've considered at length is, how can we get people using Holochain in a way where they are their own host, running their own node without even knowing or caring that they're doing it? It was actually later doing internal work for Holo that I first prototyped a solution to this, of providing a native desktop application, that used Holochain under the hood, but installed and ran without EVER touching a command line interface. Since then in my work in the hApps world, external to Holo, I've come a long way with that pattern, and one other pattern for shipping Holochain apps ("hApps") that's similarly simple. I want to share those patterns with you.

First of all, this is necessarily going to get highly technical, so I'm going to split it into part 1, which will be relatively short, and part 2, which will be quite long. Part 1 will be fine for a general audience, anyone interested, and part 2 will be a hands on reference for developers trying to make their way through the tricky waters of shipping a hApp, natively.

# Part 1, Overview

First of all, as I mentioned, the question I am answering here is one of how to get hApps developed by the blossoming Holochain developer community into the hands of people who don't use a command line, thus reaching a wide audience, and improving the scope of who can provide feedback on hApps so that they become user friendly from the get-go. Also testing and proving Holochain's viability as an accessible technology.

Under the banner of my current business venture [Sprillow](https://sprillow.com/) and on behalf of [Harris-Braun Enterprises](https://www.harris-braun.com/), we have been working on bringing Holochain apps to life. Specifically, we have been building [Acorn](https://github.com/h-be/acorn-docs), a novel approach to project management and collective visioning and achievement. This has been our laboratory for developing the patterns that I am writing about here. We have succeeded in creating releases of the Acorn software, entirely Holochain based data storage and networking, which are downloadable as pre-packaged Mac and Linux desktop applications. 

These native apps are the results we were hoping for, because they provide users with roughly a "three-click experience", that is, essentially:
1. click a download link
2. click to unzip/unarchive the download
3. click to launch

![](https://i.imgur.com/lWMFfIA.gif)

If you are a non-developer, you may have experienced some frustration if you have been around the Holochain ecosystem for a while, since the majority of links to software, at this testing stage, take you to code repositories and point you at the README.md file which has no context and drops you straight into "run x command" "run y command". My goal has been to give everybody a fair chance to play, today.

For the folks looking to understand how to get Holochain apps into those eager end-users hands short and sweetly, here's the short version...

There are two main strategies, at this time, which are viable for releasing your hApp. We can call those two strategies the two "channels", like distribution channels. For maximum cross-platform compatibility, I highly recommend building the user interface of your hApp with web stack technologies, like HTML/CSS/Javascript. You will have pathways to release to web via Holo, or to native apps, via web wrapper technologies, like [Electron](https://www.electronjs.org/).

Both the channels I want to talk about right now involve releasing your hApp as a native app. Both technically involve the use of Electron, but in different ways, so read on for those details.

## Channel 1, Holoscape
The first channel you can distribute via is called [Holoscape](https://github.com/holochain/holoscape). It's a sort of "hApp manager" for end users (and developers). There is a list of hApps hosted online that are indexed and displayed within Holoscape, for installation by end users. The Holochain runtime (Conductor) is bundled within Holoscape in this scenario, so you don't have to worry about bundling it. You just need to worry about version compatibility between a runtime decoupled from your hApp files. 

![Screen Shot 2020-05-28 at 10.24.17 AM|690x385](https://forum.holochain.org/uploads/default/original/2X/4/4e1477f71fe083616c078d2383c424f81b7125da.jpeg) 

You can see Acorn listed here, as one of the hApps available for install. It's not difficult to get your hApp listed here. Once a user installs a hApp, they can just use it windowed within Holoscape, like seen below.

![Screen Shot 2020-05-28 at 10.28.25 AM|690x392](https://forum.holochain.org/uploads/default/original/2X/b/b2faa0eca74cae4aada2a8476a8530dc144b32d3.jpeg)

Releasing to Holoscape is not too difficult. At a glance, all you have to do is:
1. [pre-compile a version of your hApp DNA file(s) and host it online](https://github.com/h-be/acorn-hc/releases/tag/v0.3.2)
2. [zip up a folder with your user interface web files and host it online](https://github.com/h-be/acorn-ui/releases/tag/v0.3.4)
3. prepare a [configuration file](https://github.com/holochain/happ-index/blob/master/happ-resources/acorn/bundle.toml) that points to these hosted assets that Holoscape can download from, and [get your hApp listed in the Holoscape index](https://github.com/holochain/happ-index/blob/master/index.json) by by submitting a pull request to the [open Github repository](https://github.com/holochain/happ-index).

In the technical deep dive I'll be showing how to do that.

I've personally found that releasing for Holoscape is a good way to get tech savvy developers on board for testing your hApp together, and less so for non tech-y end users. Holoscape provides excellent debugging tools specifically for Holochain, that can help you improve your hApp quickly and easily.

## Channel 2, "standalone"

So-called "standalone" is a reference to the initial GIF featured in this post. It is when the entire application is bundled, and that bundle includes the Holochain runtime. 

In this scenario, users download your application directly, store it in their applications folder, and run it directly off their computer with no intermediary. As mentioned, this can be achieved by using the very solid [Electron](https://electronjs.org) tool for cross-platform native apps, using web stack technology. 

At a glance, here's how this works:
1. [create a separate project from your DNA backend and your UI frontend that will serve as the Electron wrapper for your app](https://github.com/h-be/acorn-release)
2. Include [Holochain runtimes and tools](https://github.com/holochain/holochain-rust/releases) for the operating systems you want to support
3. Include your pre-built DNA and UI files into the project
4. Configure your Electron application to control the Holochain runtime
5. Use ["packaging"](https://www.electronjs.org/docs/tutorial/application-distribution) tools to build distributions
6. [Create releases and host your app files online](https://github.com/h-be/acorn-release/releases) available for download
 
As you can see, this is not such an "at a glance" thing. This work took a lot of time and dedication to work through, as someone who didn't start out with expertise in the area, and not many people do. In my opinion though, this time and energy was well worth it, and it's my hope that with shared experience the simplicity for others will be much improved.

![Screen Shot 2020-05-28 at 2.41.49 PM|629x499](https://forum.holochain.org/uploads/default/original/2X/e/ef5d4015faa46fc67f8f1410b40ca5054133b6c7.jpeg)

That's it for the overview I hope this helps give a general reader a sense of how a Holochain application can be shipped as a native app for macOS or Linux today. Next we move on to the technical deep dive outlining many tips and tricks for how to achieve all this.

<br />
<hr />


# Part 2, Technical Deep Dive

This section is intended for developers intending to try to follow one or both of the release patterns mentioned in part 1. In the case of Acorn, we decided to support both channels, feeling that they both have unique strengths and weaknesses.

Since all of the code relevant to this walkthrough is open source, I will leave the details contained there and will explain what was difficult in the process, and provide key pointers as a rough tour of the code.

The walkthrough will cover topics like: 
1. repo setup and release flow
1. developer tools & extending Holochain's [Nix](https://nixos.org/) toolchain
1. continuous integration
2. the standalone pattern
3. releasing for macOS
4. releasing for Linux
5. run your own networking "sim2h" server
6. connecting the UI to the Holochain Conductor
7. sharing UI files between standalone and Holoscape channels

## Repo Setup and Release Flow

It took us a while to pin down a good configuration for our code repositories. We started out with two repositories, [acorn-hc](https://github.com/h-be/acorn-hc) and [acorn-ui](https://github.com/h-be/acorn-ui). That refers to acorn-holochain and acorn-user-interface of course. This works well for us. This separation of concerns works especially well when it comes time to start cutting releases of your app, because there is a separate semantic versioning for the DNA than the UI. Decoupling the DNA and UI development also works well when you have a big project where separate team members are building out the DNA than the UI.

When it came to creating the standalone channel builds, we decided to introduce a third dedicated repo for hosting these releases, and the source files to build them. We called it simply [acorn-release](https://github.com/h-be/acorn-release).

You will see that no additional repository is necessarily needed for the Holoscape channel, since it is possible to simply point at the separately hosted released DNA and UI files in [acorn-hc](https://github.com/h-be/acorn-hc/releases) and [acorn-ui](https://github.com/h-be/acorn-ui/releases).

The workflow for releases looks roughly as follows, across the source projects, and the release channels:

#### Release Flow for `your-app-hc` and `your-app-ui`
1. develop locally on your-app-hc and your-app-ui till you  reach compatibility and desired features
1. cut a new release of `your-app-hc`, involves standard steps such as
  a. creating a git tag on the commit with your ready code
  b. building your app's DNA file from source
  c. creating a new release at your new git tag
  d. uploading the built DNA file as an asset to that new release
1. cut a new release of `your-app-ui`, involves standard steps such as
  a. creating a git tag on the commit with your ready code
  b. building your app's UI files from source
  c. creating a ZIP of your app's built UI files
  d. creating a new release at your new git tag
  e. uploading the built UI files ZIP as an asset to that new release

These steps can be done independently from creating new releases for Holoscape or standalone and can be done at any time.

#### Release Flow for standalone
1. From `your-app-release` repo, download from the canonical hosted release assets of `your-app-hc` and `your-app-ui`: the DNA files, and the UI files. Unzip the UI files. (This will all be automated)
1. Make any changes to the native app wrapper code required for the new version, by running it in development mode
1. cut a new release of `your-app-release`, involves standard steps such as
  a. building your app's distribution release file (should be done separately on Mac and Linux)
  b. testing your distribution release file manually
  c. creating a git tag on the commit with your ready code
  d. creating a new release at your new git tag
  e. zipping the macOS release file
  f. gzipping the Linux release files (tar.gz)
  g. uploading each of those zipped distributions to that release

#### Release Flow for Holoscape
1. Download and run Holoscape
2. Test your new version by...
  a. Creating or updating a [bundle.toml]() file which represents your app. The file should now contain references to the latest release assets of `your-app-hc` and `your-app-ui` repos.
  b. Click the Holoscape icon in your system tray and then -> `Install hApp from file`
  c. Click `Choose file` and then select your local [bundle.toml]() file from your file system
  d. Select any options you need, such as 'install with admin interface?' and `lmdb` storage
  e. Click `Install`
  f. Close the installer window, and return to the main window
  g. Navigate to your app and test it out
 1. If functioning as expected, go to the Holochain [happ-index](https://github.com/holochain/happ-index) and create a new PR to either create, or update, the `bundle.toml` file for your hApp, by [following the instructions in their README](https://github.com/holochain/happ-index#what-should-go-in-your-pr). Request a review on the PR.
1. Make any reasonable change requests that a reviewer/maintainer provides
1. Once they approve and merge it, the latest version of your hApp will show up in the "hApp list", within Holoscape.


Note that much, but not all, of this work can be automated.

These are just the overview of what the release flow actually does, and covers over the manual steps involved too, and so in the next section I'll go over developer tooling and how to achieve the automation of much of this work.

## Developer Tools & Extending Holochain's Nix Toolchain

As much as it was intimidating, getting over my aversion to familiarizing with the strange-at-first [Nix](https://nixos.org) tooling used throughout Holochain, to extend and leverage it further, became a huge blessing for eliminating (by automating) repetitive tasks.

This rabbit hole goes deep, but with so much to cover I will have to skim the surface. 

Let's recap. Nix helps developers create "reproducible builds and deployments." Reproducible is the key word. Nix enables us to have guaranteed compute environments. As a developer, you can use Nix on macOS (now with better Catalina support), other Linux distributions, and even Windows if you use [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

Because Holo decided to build Holoport OS using NixOS technology, adopting Nix tooling for developer productivity on the Holochain side made a lot of sense. This led to [Holonix](https://github.com/holochain/holonix), a foundation for developer productivity across Holo and Holochain.

Holonix has lots of useful tools built-in, but one or two things stand out and turned out most useful for our Acorn development workflow. (There is incomplete, but still relevant and useful documentation on Holonix at [https://docs.holochain.love](https://docs.holochain.love))

Here's what you'll want to know:
1. "How to Holonixify a repo", which can be found on [this page](https://docs.holochain.love/docs/configure/)
  a. This results in having a [config.nix](https://github.com/h-be/acorn-hc/blob/master/config.nix) and a [default.nix](https://github.com/h-be/acorn-hc/blob/master/default.nix) in your repo, as is seen at the links. 
1. How to [divide your extensions into separate concerns using a `/nix` folder](https://github.com/h-be/acorn-hc/tree/master/nix)
1. The last point involves knowing how to add or edit your own custom nix commands, which are essentially shell scripts. [See an example](https://github.com/h-be/acorn-hc/blob/690139b444053f5e339c2ff8ed377beb6d87ac0a/nix/release/default.nix#L6-L17). Here is another example showing how to [write multiple into one file](https://github.com/h-be/acorn-hc/blob/690139b444053f5e339c2ff8ed377beb6d87ac0a/nix/holochain/default.nix)
1. "How to upgrade Holonix", which can be found on [this page](https://docs.holochain.love/docs/configure/), but at a glance involves updating [holonix.github.ref](https://github.com/h-be/acorn-hc/blob/690139b444053f5e339c2ff8ed377beb6d87ac0a/config.nix#L17) and [holonix.github.sha256](https://github.com/h-be/acorn-hc/blob/690139b444053f5e339c2ff8ed377beb6d87ac0a/config.nix#L24) in [config.nix](https://github.com/h-be/acorn-hc/blob/master/config.nix) so that you can get the latest tools and also the latest Holochain packaged binaries
1. About the existence of, and how to use, the Holonix built-ins, such as
  a. `hc`, the Holochain developer tools
  b. `holochain`, the "production" (also development) Holochain runtime
  c.  `sim2h_server`, the networking switchboard server
  d. `github-release`, which can be used to upload assets to github releases, like [this](https://github.com/h-be/acorn-hc/blob/605cf2755ac28c2d0b623caf122b07df6336c512/nix/release/default.nix#L15-L16)
  e. `hn-release-github`, which can be used to create the release (with release notes using content templates) on github without going to github itself. This is discussed in the [README of acorn-hc](https://github.com/h-be/acorn-hc/blob/0678fba8b2b72888cd5ed38048c6fe3448f6e68c/README.md#releasing)

I hope this list can serve as a helpful reference and jumping off point for getting way more productive and accomplishing great things with nix.

## Continuous Integration
Building on the nix foundations, some of the commands you wish to run within nix will be run locally on your computer, and during development.  In other cases, the commands are best run on a continuous integration, or CI, server, such as [Travis](https://travis-ci.org/), [Circle](https://circleci.com/), or [Github Actions](https://github.com/features/actions). You can accomplish similar things with any of them, but Acorn is currently configured to work with CircleCI. The main repositories to use CircleCI right now are `acorn-hc` and `acorn-ui`, but the others may be made to use it as well. Here are the [acorn-hc Circle](https://circleci.com/gh/h-be/acorn-hc) and [acorn-ui Circle](https://circleci.com/gh/h-be/acorn-ui) locations.

By [pointing at a docker image containing Holonix](https://github.com/h-be/acorn-hc/blob/b12992680e16ef4c1e5050ccea907932a1ea5277/.circleci/config.yml#L6), it is easy to get your nix commands running on a CI server. The Acorn pattern utilizes this for the purposes of two things:
1. [running automated tests](https://github.com/h-be/acorn-hc/blob/b12992680e16ef4c1e5050ccea907932a1ea5277/.circleci/config.yml#L7-L16). It runs the testing command [for any git branch on github](https://github.com/h-be/acorn-hc/blob/b12992680e16ef4c1e5050ccea907932a1ea5277/.circleci/config.yml#L29-L31) and shows those results on the Github Pull Requests "status checks" feature
![Screen Shot 2020-06-30 at 8.06.25 AM|690x142, 75%](https://forum.holochain.org/uploads/default/original/2X/5/578edf538a0d2165bc1b5bf816418fd68cc855dc.png) 
1. [building and uploading assets for releases](https://github.com/h-be/acorn-hc/blob/b12992680e16ef4c1e5050ccea907932a1ea5277/.circleci/config.yml#L18-L25). It runs this command [for any git tag, but no branches](https://github.com/h-be/acorn-hc/blob/b12992680e16ef4c1e5050ccea907932a1ea5277/.circleci/config.yml#L33-L40)
![Screen Shot 2020-06-30 at 8.14.44 AM|690x339, 50%](https://forum.holochain.org/uploads/default/original/2X/e/e44638784864aec6df15a47925c6cca0ed03b36f.png) 

In the case of testing, the end goal is confidence in your code, which you can now have. In the case of releases, the goal is simplicity, and consistency. By automating the workflow, you eliminate the likelihood of human error or inconsistency. An example of a release created by this workflow is seen here, in [v0.3.2. release](https://github.com/h-be/acorn-hc/releases/tag/v0.3.2) of `acorn-hc`. The assets in this case are two distinct DNA files that are built from project source code.

> Please note that if you want to set up this workflow, you will need to follow certain steps, including the setup of an oauth token that `github-release` can use to authenticate to Github. 
You must [get an oauth token](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) for a user with write access to the repository in question. You must then set an environment variable for `GITHUB_DEPLOY_TOKEN` on your CI server equal to that token. You can see where it is used at the end of [these two lines](https://github.com/h-be/acorn-hc/blob/690139b444053f5e339c2ff8ed377beb6d87ac0a/nix/release/default.nix#L15-L16). If you use the Holonix `hn-release-github` built-in command you will also want to set up a git config setting, with that same token, or a different one also with write access. The git config setting to use is `hub.oauthtoken`. To set that as a local configuration for a repository, run 
```git config --local --add hub.oauthtoken X123MYTOKEN```



For `acorn-release`, partial testing automation has been set up, but "release" automation has not. The reason for that is yet to come, but as a summary it will suffice to say that special system configurations for building for macOS, and the desire to manually quality assure releases, keeps us manually releasing our native app builds. Let's get into that.


## The Standalone Pattern

Let's recall what the "standalone pattern" is. It is a desktop based application that will run natively on a users computer. It will be managed from their applications bar. The last few years have seen the rise of particular solution for writing one codebase and still targeting multiple platforms, and that solution is called [Electron](https://www.electronjs.org/). Provided that without Holochain currently  having a Windows release, we can currently only target macOS and Linux platforms.

Let's take a closer look at wrapping a Holochain application in an Electron wrapper.

We created a standalone repository for hosting the Electron wrapper source code, that we called [acorn-release](https://github.com/h-be/acorn-release). The key ideas for the wrapper are that
- [The Electron process manages the private keys that a user needs, by creating a key for them, if they don't have one already](https://github.com/h-be/acorn-release/blob/08ee1ea4b526a97397bc51a7be035ed9cfd33325/main.js#L223-L254)
- The Electron process manages the Holochain ["conductor configuration file"](https://github.com/h-be/acorn-release/blob/8067d1d25a4ba23b5aacda55869f1fc716a71fdb/conductor-config.toml), [creating it if it hasn't been created, and passing it the right values dynamically when it creates it](https://github.com/h-be/acorn-release/blob/08ee1ea4b526a97397bc51a7be035ed9cfd33325/main.js#L108-L158)
- [The Electron process manages Holochain as a background process, while the app itself is running](https://github.com/h-be/acorn-release/blob/08ee1ea4b526a97397bc51a7be035ed9cfd33325/main.js#L162-L202). If the app is closed, Holochain isn't running.
- [The Holochain binaries for either Mac, or Linux, are directly embedded into the application folder, but gitignored](https://github.com/h-be/acorn-release/blob/08ee1ea4b526a97397bc51a7be035ed9cfd33325/.gitignore#L18-L20)
- [The Holochain binaries are fetched from the Github releases of Holochain, and that is done automatically when building for a specific platform](https://github.com/h-be/acorn-release/blob/8067d1d25a4ba23b5aacda55869f1fc716a71fdb/nix/acorn/default.nix#L81-L95)
- The Holochain app DNA(s) are fetched from the [releases on Github for acorn-hc](https://github.com/h-be/acorn-hc/releases) and [bundled into the app folder, and the hash addresses of the fetched DNAs are calculated at that time](https://github.com/h-be/acorn-release/tree/8067d1d25a4ba23b5aacda55869f1fc716a71fdb#acorn-hc)
- The HTML/CSS/JS files for the user interface are fetched from the [releases on Github for acorn-ui](https://github.com/h-be/acorn-hc/releases) and [bundled into the app folder](https://github.com/h-be/acorn-release/tree/8067d1d25a4ba23b5aacda55869f1fc716a71fdb#acorn-ui)
- [A custom protocol scheme is used to serve the UI files, instead of using the default 'file' based protocol](https://github.com/h-be/acorn-release/blob/8067d1d25a4ba23b5aacda55869f1fc716a71fdb/main.js#L47-L88)
- [The ui uses the hidden `_dna_connections.json` path that the Holochain conductor uses to figure out which port to connect to the main Holochain websocket at](https://github.com/h-be/acorn-release/blob/08ee1ea4b526a97397bc51a7be035ed9cfd33325/conductor-config.toml#L67-L79)
- [Use the 'electron-packager' nodejs utility to package the app into distributions](https://github.com/h-be/acorn-release/blob/8067d1d25a4ba23b5aacda55869f1fc716a71fdb/nix/acorn/default.nix#L94)


I recommend forking the `acorn-release` code if you attempt to set all of this up, as it will give you the best head start on having all that configured. 

## Releasing for macOS

This section will discuss the challenges and solutions of releasing for macOS. What we discovered is that providing first-class support for macOS is significantly harder than providing the same for Linux. This is because of far stricter requirements that Apple has, than the open source Linux community, and additional costs/complexity.

The basic requirements for achieving the build for macOS are that:
- [You must pay for an Apple developer account (currently $99 USD / year)](https://developer.apple.com/programs/enroll/)
    - We know that paying to distribute your app is a pain, but it does provide end users a much better experience in the end. It is the difference between their seeing "This app is from an unidentified developer" popping up when they attempt to launch your app, or it launching cleanly without a strange warning like that. You will save yourselves lots of work by not trying to distribute through the app store, but you will also have to self-host your releases, on Github or otherwise. 
- [You must attain a Developer ID certificate, the kind that is appropriate for "distribution outside the app store" (unless you planned to distribute through the app store in which case this tutorial won't be as useful)](https://developer.apple.com/support/developer-id/)
    - Use Apple's documentation once you are signed in to determine how to install the certificate to your device. Building an app for Mac can only be done on a Mac, so yes you will need a Mac to be able to achieve all of this at all. The building won't be able to be easily automated on a cloud hosted machine, since you shouldn't  and probably can't install a certificate to a cloud device.
- You set up the proper ['entitlements'](https://developer.apple.com/documentation/bundleresources/entitlements) for your app, which you can base off this [linked file](https://github.com/h-be/acorn-release/blob/master/entitlements.mac.plist)
    - The entitlements are basically an indicator to the macOS system of which capabilities this app needs from the operating system. Every one of the entitlements we used in the entitlements file will be necessary for any Holochain app running like Acorn, apart from one, which is the "com.apple.security.files.user-selected.read-write", since we download files from the app onto the device, into a user specified location. The rest can be all looked up at the linked "entitlements" link in the initial point
- [You set up the right environment variables on a computer to keep your identity secure, which are used in the build process](https://github.com/h-be/acorn-release#packaging)
    - By setting these environment variables we enable the automated scripts to access your account, and get your build "signed" and "notarized", which we'll discuss in a minute.

Having done all these things, you will be able to utilize the `acorn-release` pattern for codesigning and notarizing your app. This will allow your end users to trust the app they are receiving greater than they would be able to otherwise. 

You need to run three commands to build a release.
```nix
nix-shell --run acorn-bundle-dna
nix-shell --run acorn-bundle-ui
nix-shell --run acorn-build-mac
```

Concretely speaking, what happens is that when you run the command that uses electron-packager to create a new release of your app, `nix-shell --run acorn-build-mac`, it will not only create the app archive file that you can distribute, but digitally sign it using the certificate on your device, and then notarize it, which involves uploading the file to Apple's servers, and waiting for it to crunch the code to look for any faults in the source code, or anything suspicious about the entitlements, or otherwise. This usually takes about 10 minutes of waiting for that process to complete. 

In order to see the full logs relating to signing and notarizing logs, while you run this, make sure to set this environment variable before running `acorn-build-mac`, to enable logging for the underlying nodejs libraries that handle those things:
```
DEBUG=electron-osx-sign*,electron-notarize*
```

In the screenshot below, this environment variable was not set. Notice how if you are simply within the nix-shell at the time, you can just run the command `acorn-build-mac`, instead of the full `nix-shell --run acorn-build-mac` that you need to run if you aren't in the nix-shell already.

![A screenshot that displays the command acorn-build-mac being run, and then all the logs that follow, ending in "Wrote new app to /Users/x/code/hbe/acorn-release/Acorn-darwin-x64"](https://i.imgur.com/S349jZ5.png)


Once it does, you will want to test the file before uploading it as a release anywhere. I recommend testing on a secondary computer to the one that the app was built on, because that computer will automatically trust the app, whereas another persons Mac won't. So send it to another person, and have them try to launch the app, and verify that it works, and that you don't get an 'unidentified developer' message or a different bug.

At this point, I usually zip the file, and name that zip something like "Acorn-darwin-x64_v0.3.3.zip". `Acorn` is the name of the app, `darwin` is the codename for macOS architectures, `x64` means 64-bit processors, and `v0.3.3` is the version number of this release, which changes each time. 

Draft the new release, and upload the zip file to the release. You will want to make sure that you get the Linux release ready as well before proceeding to actually publish the release. 

> Please note ... major issues were encountered when attempting to publish the first releases of Acorn with built-in Holochain, and having it be codesigned and notarized. This was due to issues with "dynamic linking" of referenced dependencies for the Holochain binaries, that are being distributed on Github. The source code that manages fixing this issue is found [here](https://github.com/h-be/acorn-release/blob/08ee1ea4b526a97397bc51a7be035ed9cfd33325/nix/acorn/default.nix#L62-L79). It may be necessary at times, to update this code for future versions, which represents a major catch. Use the `otool` utility mentioned there to figure out what changes need to be made.

## Releasing for Linux

Building the Linux release is as simple as running 
```nix
nix-shell --run acorn-bundle-dna
nix-shell --run acorn-bundle-ui
nix-shell --run acorn-build-linux
```

Give the resulting app file a quick manual test.
Compress that file using gzip, and name it something like `Acorn-linux-x64_v0.3.3.tar.gz`. 
Upload that file to the release draft, along with the macOS distribution, write your release notes, and publish it!

## Run your own networking "sim2h" server

In my experience, if you are trying to move an app from development into production, you want maximum control over any related infrastructure necessary for the proper functioning of your application. In the case of Acorn, this led us to self-host a version of the "switchboard" networking server that current versions of Holochain are still dependent on, called "sim2h". Sim stands for "simulated". What this means right now is that in order for Holochain to have proper functioning each new user of your application will transmit messages to other peers via the sim2h server. End users get the best user experience from this architecture, while Holochain developers work to implement full peer-2-peer architecture. Here's what we did to run our own sim2h. 

Using a server owned by [Harris-Braun Enterprises](https://www.harris-braun.com/), we set up a version of sim2h for each version of sim2h/Holochain that gets released that we also  release upon.

Compatible versions of sim2h are released as binaries with each new release version of Holochain. In [this release](https://github.com/holochain/holochain-rust/releases/tag/v0.0.51-alpha1) for example, you can find a Linux version of sim2h as file "sim2h_server-v0.0.51-alpha1-x86_64-generic-linux-gnu.tar.gz" under the release assets.

To run the sim2h server on a specific port, when you have the binary, run something like this:

```nix
sim2h_server --port 9051
```

Make sure that any firewall you have for the server allows traffic through port 9051, or whatever port you run the process on.

sim2h uses standard `RUST_LOG` logs, meaning you can write the logs to stdout by setting `RUST_LOG=debug` as an environment variable before running the `sim2h_server` command. 

In order to have our DNAs reliably connect to the `sim2h` server that we run, we've just adopted the strategy of embedding a specific `sim2h_url` property into the properties objects of the two DNAs we build. We set this as follows in the [app.json files](https://github.com/h-be/acorn-hc/blob/b72a8b9e1899223f4e7a79d9791ed91dae8445c9/dnas/profiles/app.json#L14) at the root of a DNA source code folder:
```json
  ...
  "properties": {
      "sim2h_url": "ws://sim2h.harris-braun.com:9051/"
  }
  ...
```

The last two digits of the port number here are a hidden reference to associated holochain/sim2h release number, e.g. 9051 = 0.0.51(-alpha1).

Setting this property will mean that any Holochain Conductor, if instructed to use sim2h for networking, will look for that URL in the properties of a DNA, and use it preferentially over whatever Conductor-wide setting for sim2h_url was set. The ability to do this was introduced in [this pull request](https://github.com/holochain/holochain-rust/pull/1828). This is good for us because it means that when run within Holoscape, the DNA will use its own preferred URL as a sim2h connection.

The only catch to this is that embedding this property in the DNA can cause issues for your tests (and did for ours), unless you set a special environment variable when you run your tests, to use whatever URL you would prefer for testing purposes. We had to [add a line to our testing command](https://github.com/h-be/acorn-hc/blob/b72a8b9e1899223f4e7a79d9791ed91dae8445c9/nix/test/default.nix#L7) to make sure it worked properly.
```nix
  HC_IGNORE_SIM2H_URL_PROPERTY=true hc test --skip-package
```

## Handling Variable DNA Addresses

There was a challenge that we encountered throughout the development process, regarding the fact that anytime you change the DNA source code in [acorn-hc](https://github.com/h-be/acorn-hc/tree/master/dnas), the calculated DNA address of that DNA changes, which impacts both other repositories. In order to properly function, both `acorn-ui` and `acorn-release` need up-to-date references. In both cases, the problems arise not during development, where you just run `acorn-hc` as a backend, but when packaging it all up for production.


### acorn-release 
For `acorn-release`, when the [Holochain conductor launches](https://github.com/h-be/acorn-release/blob/08ee1ea4b526a97397bc51a7be035ed9cfd33325/main.js#L163), and gets configured based off its prepopulated `conductor-config.toml`, it needs [this line](https://github.com/h-be/acorn-release/blob/08ee1ea4b526a97397bc51a7be035ed9cfd33325/conductor-config.toml#L16) to have been replaced with the actual exact matching address of the DNA file that it is trying to run, in particular for the [profiles.dna.json](https://github.com/h-be/acorn-release/blob/08ee1ea4b526a97397bc51a7be035ed9cfd33325/conductor-config.toml#L14). Note that `hash = ...` refers to the same concept that I mean when I say `address`, which is used elsewhere throughout Holochain. 

Here are the steps we had to take to get an automated system for keeping that line up to date:
1. When calling custom nix command `acorn-bundle-dna`, which fetches a released version of `acorn-hc` DNAs, it uses the `hc` developer utility tool called `hash` to calculate the address/hash for both DNAs, and saves the results (after cleaning the raw terminal output) to an independent file for each, containing only the current addresses/hashes for the DNAs
    `hc hash --path dna/profiles.dna.json | awk '/DNA Hash: /{print $NF}' | tr -d '\n' > profiles_dna_address`
    The names of the files are `profiles_dna_address` and `projects_dna_address`, where "projects" and "profiles" are the given names for the modularized `acorn-hc` functionality.
2. [Set up the `conductor-config.toml` with an placeholder line for `hash = ''`, and a comment noting this will be a replaced config property](https://github.com/h-be/acorn-release/blob/3ae335adffdddf1547a2d69b711ae355458157ec/conductor-config.toml#L15-L16)
3. During the `updateConductorConfig()` function of the Electron app, make sure that a whole new `conductor-config.toml` file is written to the Electron [appData](https://github.com/h-be/acorn-release/blob/3ae335adffdddf1547a2d69b711ae355458157ec/main.js#L25) folder of the device, while will vary according to operating system
4. [Read the address value from the `profiles_dna_address` file](https://github.com/h-be/acorn-release/blob/3ae335adffdddf1547a2d69b711ae355458157ec/main.js#L111-L113), then do a [regex replace](https://github.com/h-be/acorn-release/blob/3ae335adffdddf1547a2d69b711ae355458157ec/main.js#L135-L139) of `hash = ''` in the new `conductor-config.toml` that you are creating, with that value
5. [Add the *_dna_address text files to .gitignore](https://github.com/h-be/acorn-release/blob/3ae335adffdddf1547a2d69b711ae355458157ec/.gitignore#L8-L10)

### acorn-ui

For `acorn-ui`, which uses special admin level control over the Holochain Conductor, we needed the ability to [install a new DNA clone of our `projects.dna.json` during runtime](https://github.com/h-be/acorn-ui/blob/master/src/projects/conductor-admin/actions.js#L25-L34). In order to accomodate Holoscape, the DNA would actually have a path like `123asdfjk23.dna.json`, where `123asdfjk23` is the address of the DNA. For that reason, `acorn-ui` had to know during build/compile-time the address. Once again, during development, this wasn't necessary, so we folded it into the build step for `acorn-ui`.

Here are the steps we took to keep the "projects" DNA address a perfect match with the actual bundled file for Holoscape and `acorn-release`:
1. During the nix command `acorn-build` in `acorn-ui`, [we fetch the DNA file from the relevant `acorn-hc` Github release](https://github.com/h-be/acorn-ui/blob/44755e87e353ac018da6b573b0fbef91f5e9e04a/nix/release/default.nix#L6). The version number is very important. It can be set within the `acorn-build` command, or passed as an override to the command, e.g. `nix-shell --run 'acorn-build 0.3.5'`
```
curl -O -L https://github.com/h-be/acorn-hc/releases/download/v''${1:-0.3.4}/projects.dna.json
```
2. We [use the same `hc hash` utility to calculate the DNA address for the DNA file to  set an environment variable `PROJECTS_DNA_ADDRESS`](https://github.com/h-be/acorn-ui/blob/44755e87e353ac018da6b573b0fbef91f5e9e04a/nix/release/default.nix#L7)
```
 export PROJECTS_DNA_ADDRESS="'$(hc hash --path projects.dna.json | awk '/DNA Hash: /{print $NF}' | tr -d '\n')'"
```
3. We perform our [npm run build](https://github.com/h-be/acorn-ui/blob/44755e87e353ac018da6b573b0fbef91f5e9e04a/nix/release/default.nix#L8) command, which [uses webpack to create our production UI assets build](https://github.com/h-be/acorn-ui/blob/44755e87e353ac018da6b573b0fbef91f5e9e04a/package.json#L8)
```
 ${pkgs.nodejs}/bin/npm run build
```
4. Our [webpack.prod.js file](https://github.com/h-be/acorn-ui/blob/master/webpack.prod.js) will use [`webpack.DefinePlugin` to find and replace the text `__PROJECTS_DNA_ADDRESS__` in the source code with the value of the given `PROJECTS_DNA_ADDRESS` environment variable](https://github.com/h-be/acorn-ui/blob/44755e87e353ac018da6b573b0fbef91f5e9e04a/webpack.prod.js#L20-L22)
5. [A variable in the code is set equal to this replaced value of `__PROJECTS_DNA_ADDRESS__`](https://github.com/h-be/acorn-ui/blob/44755e87e353ac018da6b573b0fbef91f5e9e04a/src/holochainConfig.js#L35) and then [utilized](https://github.com/h-be/acorn-ui/blob/44755e87e353ac018da6b573b0fbef91f5e9e04a/src/holochainConfig.js#L38)


As you may have gathered, the real key to all of this is the `hc hash` capability of the `hc` Holochain developer tools. This takes a DNA file as an input, and returns the exact same hash address that the Holochain Conductor `holochain` will calculate, when it reads that file in, and performs verification on it (throwing a "DNA hash mismatch" error if it fails). The only catch to that command is that it returns a human-readable (vs machine-readable) string, so it needs to be cleaned up of that "DNA Hash" noise from the output, and stripped of its trailing newline character.
```nix
hc hash --path dna/profiles.dna.json | awk '/DNA Hash: /{print $NF}' | tr -d '\n'
```

## Connecting the UI to the Holochain Conductor

Let's revisit a fundamental of Holochain. When you run a Holochain Conductor, `holochain`, it can expose multiple interfaces at which the operations encoded in a DNA can be called, and those interfaces can either be websockets or HTTP. In almost every case I have seen developers have chosen websockets, which offer many benefits over the HTTP interface. A websocket has to run on a free networking port on the device it is running on. 

It is a common choice, [and we chose this with Acorn](https://github.com/h-be/acorn-ui/blob/44755e87e353ac018da6b573b0fbef91f5e9e04a/src/index.js#L42), to utilize the [hc-web-client](https://github.com/holochain/hc-web-client) library for connecting between an HTML/CSS/JS user interface, and a Holochain Conductor. The library is optimized for that use case.

Of course, the HTML/CSS/JS files are going to have to be served one way, on one port, and the DNA interface is going to have to be run on a different port, so how will the JS files, and `hc-web-client` know which path to connect to. Holochain had an answer for this, unfortunately it is in a state of basically deprecation, but it was the best solution I could find. 

The original answer was to serve a route at the path `_dna_connections.json` over the same interface as UI files were being served, which would tell `hc-web-client` where (which port) an active DNA interface was being served, which it would automatically go and connect to. Holochain doesn't now recommend serving UI files that way, but still has assumptions about `_dna_connections.json` built in to `hc-web-client`. Thus, we needed to lots of work to accomodate this. 

For reference, a `_dna_connections.json` response looks like this:
```json
{
  "dna_interface": {
    "id": "Acorn-interface",
    "driver": {
      "type": "websocket",
      "port": 8888
    },
    "admin": true,
    "instances": [
      {
        "id": "acorn-profiles-instance",
        "alias": null
      }
    ],
    "choose_free_port": null
  }
}
```


### acorn-ui and acorn-hc

In `acorn-ui`, the problem mainly arises during development mode, because, for example, the files are being served over port 8000, and the DNA interface is running on port 8888, how will the UI know where to connect to the websocket, if we don't explicitly pass 8888 in as a value to it, which we are looking to avoid with `hc-web-client`'s `connect` function.

Fortunately, the solution in this case came as a webpack option.
If we set up in `acorn-hc` `starter.conductor-config.toml` template a ["ui bundles" and "ui interfaces" option (which are basically a mock since they won't serve any real files, only the `_dna_connections.json` path)](https://github.com/h-be/acorn-hc/blob/06f7f0dbe54e33f96142275b87f1ac8bb7aab7b3/starter.conductor-config.toml#L35-L46) and run it on port 3111, then we can specify in the [webpack config for development mode, to proxy that particular path from port 8000 to port 3111](https://github.com/h-be/acorn-ui/blob/44755e87e353ac018da6b573b0fbef91f5e9e04a/webpack.dev.js#L9-L11). Here's the lines:
```
  ...
  proxy: {
    '/_dna_connections.json': 'http://localhost:3111',
  },
  ...
```

That solves our problem for development mode, because the `hc-web-client` `connect` function in `acorn-ui` hits the port 8000 `/_dna_connections.json` path, gets proxied through to the Holochain  http server at port 3111, and finds in the JSON response a port 8888 at which to connect to the DNA websocket interface. Bingo. 

### acorn-release

Without the proxy option in `acorn-release`, due to no `webpack`, we needed another way to serve `/_dna_connections.json`. 

As a start, [we enabled the same "ui bundles" and "ui interfaces" section of the `conductor-config.toml` template](https://github.com/h-be/acorn-release/blob/3ae335adffdddf1547a2d69b711ae355458157ec/conductor-config.toml#L67-L79) as we did in `acorn-hc`.

In `acorn-release` we use a special system built in to Electron to serve the UI files, a customized `FileProtocol`. What it achieves in practice is control over the browsers requests, when the requests come over that protocol, such as `acorn-protocol://root`.

To pull this off, you have to:
1. [define the protocol string](https://github.com/h-be/acorn-release/blob/3ae335adffdddf1547a2d69b711ae355458157ec/main.js#L47)
2. [Register privileges to the protocol scheme](https://github.com/h-be/acorn-release/blob/3ae335adffdddf1547a2d69b711ae355458157ec/main.js#L48-L58)
3. [Define a handler for requests over the protocol, which respond with a file](https://github.com/h-be/acorn-release/blob/3ae335adffdddf1547a2d69b711ae355458157ec/main.js#L60-L75)
4. [On Electron app 'ready' event (and not before), call `registerFileProtocol` with the scheme and the handler](https://github.com/h-be/acorn-release/blob/3ae335adffdddf1547a2d69b711ae355458157ec/main.js#L204-L214)

Since we already needed this protocol scheme, we found we could use it to our advantage to serve the `/_dna_connection.json` path. It would need to provide the response as a file.

[My solution was to catch and override requests from the UI for `ui/_dna_connections.json` and provide the path to a file containing the relevant JSON response.](https://github.com/h-be/acorn-release/blob/3ae335adffdddf1547a2d69b711ae355458157ec/main.js#L69-L70)

Now that file needed to exist. [To achieve that, on initial launch, we used an http request to the booted Holochain Conductor to capture the JSON response at `/_dna_connections.json`, and write it to the file that the previous section would return the path of.](https://github.com/h-be/acorn-release/blob/3ae335adffdddf1547a2d69b711ae355458157ec/main.js#L172-L187) [To make the HTTP call at the right time, we needed to listen to the log output of the Holochain Conductor.](https://github.com/h-be/acorn-release/blob/3ae335adffdddf1547a2d69b711ae355458157ec/main.js#L170-L172)

To be clear, this wasn't a solution that I was at all happy about, but on discovering that [Holoscape had its' own odd approach to `_dna_connections.json`](https://github.com/holochain/holoscape/blob/e58d5141022a78cbe47faa314560cf83b20a5c74/happ-ui-controller.js#L54), I knew I had no other real choice. I essentially based this solution on that one.

With all of that in place, the embedded `acorn-ui` files being served by Electron began to run great, and `hc-web-client` could make the connection to whatever port the DNA interface was on that it had running!


## Sharing UI Files Between standalone and Holoscape Channels

Overall, it was our goal to share the `acorn-ui` src files completely between the `acorn-release`/Electron, and Holoscape (also Electron) channels. The fact that Holoscape and our native build process were both Electron has definitely played to our advantage. 

After unifying as much as possible about our backend configuration with Holoscape's, there was only one place in the UI code where we needed to distinguish between the two contexts, which was great.

There is a special trick to be able to directly access the Electron API from the client. [It looks like this](https://github.com/h-be/acorn-ui/blob/44755e87e353ac018da6b573b0fbef91f5e9e04a/src/holochainConfig.js#L15):
```javascript
const { app } = window.require('electron').remote
```

This `app` object contains properties that are set in the `package.json` of the app. [This is why we can access `app.name` to check whether we are in Holoscape or the standalone.](https://github.com/h-be/acorn-ui/blob/44755e87e353ac018da6b573b0fbef91f5e9e04a/src/holochainConfig.js#L19-L20)

Since the "appData" folders for the two contexts are different, we know to look in either [`Holoscape-default` or `Acorn`](https://github.com/h-be/acorn-ui/blob/44755e87e353ac018da6b573b0fbef91f5e9e04a/src/holochainConfig.js#L19-L24).

# In Conclusion

Releasing production ready software is always a challenge, and when you are building on cutting edge technology like Holochain, the challenges can be more numerous, but not impossible to overcome, as we have shown! We hope that this article, and the open source release of all Acorn code, helps you to achieve a great outcome at a quick pace. Please feel free to reach out to us if you need help, or guidance. We also welcome participation in the project, so if you're interested, [learn more about it](https://github.com/h-be/acorn-docs), and chat with us on the [Holochain forum](https://forum.holochain.org/c/projects/acorn/106) or [live chat channels in Mattermost](https://chat.holochain.org/appsup/channels/acornstateofaffairs).

You can download the Acorn prototype for Mac or Linux today
- [from the latest standalone release](https://github.com/h-be/acorn-release/releases)
- [or from the "hApp list" of the latest Holoscape release](https://github.com/holochain/holoscape/releases)


You can reach me personally at `connor at sprillow dot com`.

