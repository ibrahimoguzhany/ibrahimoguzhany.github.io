---
id: 83287
title: Adding Splash Screen Animation to Default Flutter App
date: 2020-03-24T12:50:47+00:00
author: Fercan Şen
layout: post
guid: https://gbot.dev/?p=83287
permalink: /2020/03/24/adding-splash-screen-animation-to-default-flutter-app/
categories:
  - Uncategorized
---
## Introduction

In this blog post, we are going to make a splash screen animation using Rive (formerly known as Flare). One of the benefits of using Rive is that it reduces boilerplate code when creating animations. First, we are going to design our animation, then we are going to implement it to the default flutter project. We will cover this in two parts. One being design of the animation and the other is implementation.

## Part 1: Design and Animate

Go to rive.app, create your account. Then create a new flare project. This what it will look like:

<img class="alignnone wp-image-83307 size-full" src="https://gbot.dev/wp-content/uploads/2020/03/Default-Flare-1.png" alt="" width="1918" height="903" srcset="https://gbot.dev/wp-content/uploads/2020/03/Default-Flare-1.png 1918w, https://gbot.dev/wp-content/uploads/2020/03/Default-Flare-1-300x141.png 300w, https://gbot.dev/wp-content/uploads/2020/03/Default-Flare-1-1024x482.png 1024w, https://gbot.dev/wp-content/uploads/2020/03/Default-Flare-1-768x362.png 768w, https://gbot.dev/wp-content/uploads/2020/03/Default-Flare-1-1536x723.png 1536w" sizes="(max-width: 1918px) 100vw, 1918px" /> 

After you drag and drop your assets, position your design as you desire. Then proceed to the animation tab. We can animate almost anything on the shape.

<img class="alignnone wp-image-83308 size-full" src="https://gbot.dev/wp-content/uploads/2020/03/design.png" alt="" width="1920" height="900" srcset="https://gbot.dev/wp-content/uploads/2020/03/design.png 1920w, https://gbot.dev/wp-content/uploads/2020/03/design-300x141.png 300w, https://gbot.dev/wp-content/uploads/2020/03/design-1024x480.png 1024w, https://gbot.dev/wp-content/uploads/2020/03/design-768x360.png 768w, https://gbot.dev/wp-content/uploads/2020/03/design-1536x720.png 1536w" sizes="(max-width: 1920px) 100vw, 1920px" /> 

### Some useful hotkeys when animating:

  * **T** for translate, move shapes around.
  * **S** for scaling the shapes.
  * **R** for rotating the shapes.
  * **ctrl+z** to undo

When you are done with animating export your .flr file in binary format.

<img class="aligncenter wp-image-83317 size-full" src="https://gbot.dev/wp-content/uploads/2020/03/export-1.png" alt="" width="435" height="422" srcset="https://gbot.dev/wp-content/uploads/2020/03/export-1.png 435w, https://gbot.dev/wp-content/uploads/2020/03/export-1-300x291.png 300w" sizes="(max-width: 435px) 100vw, 435px" /> 

## Part 2: Flutter Integration

Rive provides a package that makes it really simple to use our animations into an iOS or Android app with Flutter. First, create a default Flutter project. You can do that following the steps below. We are using VS Code as IDE.

To create a new Flutter project from the Flutter starter app template:

  1. Open the Command Palette (<code class="highlighter-rouge">Ctrl</code>+<code class="highlighter-rouge">Shift</code>+<code class="highlighter-rouge">P</code> (<code class="highlighter-rouge">Cmd</code>+<code class="highlighter-rouge">Shift</code>+<code class="highlighter-rouge">P</code> on macOS)).
  2. Select the **Flutter: New Project** command and press <code class="highlighter-rouge">Enter</code>.
  3. Enter your desired **Project name**.
  4. Select a **Project location**.

<p id="opening-a-project-from-existing-source-code">
  Now your default project should be ready.
</p>

<p style="text-align: center;">
  Add flare_splash_screen to the pubspec.yaml, under dependencies.
</p>

<img class="aligncenter wp-image-83311 size-full" src="https://gbot.dev/wp-content/uploads/2020/03/dependenies.png" alt="" width="313" height="113" srcset="https://gbot.dev/wp-content/uploads/2020/03/dependenies.png 313w, https://gbot.dev/wp-content/uploads/2020/03/dependenies-300x108.png 300w" sizes="(max-width: 313px) 100vw, 313px" /> 

<p style="text-align: center;">
  Create an assets folder within your flutter project and add the .flr file you exported to assets.
</p>

<img class="aligncenter wp-image-83313 size-full" src="https://gbot.dev/wp-content/uploads/2020/03/assets.png" alt="" width="337" height="487" srcset="https://gbot.dev/wp-content/uploads/2020/03/assets.png 337w, https://gbot.dev/wp-content/uploads/2020/03/assets-208x300.png 208w" sizes="(max-width: 337px) 100vw, 337px" /> 

<p style="text-align: center;">
  Now go and activate your .flr file in pubspec.yaml. Just type the path of the file under assets.
</p>

<p style="text-align: center;">
  <img class="alignnone wp-image-83312 size-full" src="https://gbot.dev/wp-content/uploads/2020/03/assets-dependencies.png" alt="" width="746" height="137" srcset="https://gbot.dev/wp-content/uploads/2020/03/assets-dependencies.png 746w, https://gbot.dev/wp-content/uploads/2020/03/assets-dependencies-300x55.png 300w" sizes="(max-width: 746px) 100vw, 746px" />
</p>

Saving something in pubspec.yaml file automatically runs &#8220;flutter pub get&#8221; in VSCode. If it does not <span style="font-weight: 400;">you can type </span><span style="font-weight: 400;">flutter pub get</span> <span style="font-weight: 400;">in the terminal.</span>

After adding dependencies and assets we need to import the package in the main.dart like so.

<img class="wp-image-83320 size-full aligncenter" src="https://gbot.dev/wp-content/uploads/2020/03/import.png" alt="" width="666" height="62" srcset="https://gbot.dev/wp-content/uploads/2020/03/import.png 666w, https://gbot.dev/wp-content/uploads/2020/03/import-300x28.png 300w" sizes="(max-width: 666px) 100vw, 666px" /> 

Then we go to our main section.

<img class="wp-image-83321 size-full aligncenter" src="https://gbot.dev/wp-content/uploads/2020/03/main.png" alt="" width="747" height="486" srcset="https://gbot.dev/wp-content/uploads/2020/03/main.png 747w, https://gbot.dev/wp-content/uploads/2020/03/main-300x195.png 300w" sizes="(max-width: 747px) 100vw, 747px" /> 

In &#8220;home&#8221; we give SplashScreen.navigate widget.

  * Name  -> We type the destination of your .flr file.
  * Next    -> Give the next widget to be displayed. In this case, we navigate to the homepage.
  * Until    -> Duration of the animation on the screen.
  * startAnimation  -> Name of the animation. It is &#8220;Untitled&#8221; by default. You can change this on rive.app.
  * backgroundColor -> Background color of the splash screen.
  * fit -> This makes the animation fit the whole screen.

GitHub repo of the project: <https://github.com/FercanSen/gbot-animation>