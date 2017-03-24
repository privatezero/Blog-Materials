# Professional Development Time Project: Audiorecorder
### Resident: Andrew Weaver, CUNY TV

One of the incredible things about my NDSR residency is that it requires me to spend 20% of my time working on professional development.  This allows me to devote quite a bit of time to learning skills and working on things of interest that would otherwise be outside of the scope of my project.  (For people who are considering applying to future NDSR cohorts, I can’t emphasize enough how great it is to have the equivalent of one day a week to focus your attention on growth in areas of your choosing).

A great way to spend some of this time was suggested to me through some personal audio projects I am working on.  I realized that, A: I wanted a tool that would allow me a range of signal monitoring capabilities while digitizing audio. B: I didn’t feel like paying for it. Solution: Try to build one myself!

As I was familiar with using [vrecord](https://github.com/amiaopensource/vrecord) for video digitization, I decided to build a tool called ‘audiorecorder’ that followed the same approach- namely using a shell script to pipe data between FFmpeg and ffplay.  This allows capturing data in high quality, while simultaneously visualizing it.  In looking at the filters available through FFmpeg, I decided to build an interface that would allow monitoring of peak levels, spectrum and phase.  This seemed doable and would give me pretty much everything I would need for both deck calibration and QC while digitizing.  The layout I settled on looks like:

![audiorecorder interface](https://github.com/privatezero/Blog-Materials/raw/master/audiorecorder.gif)

I also wanted audiorecorder to support embedding BWF metadata and to bypass any requirement for opening the file in a traditional DAW for trimming of silence.  After quite a bit of experimentation, I have hit upon a series of looped functions using FFmpeg that allow for files to be manually trimmed both at the start and at the end, with the option for auto silence detect at the start.  Being able to do this type of manipulation makes it possible to embed BWF metadata without worrying about having to re-embed in an edited file.

<img src="https://github.com/privatezero/Blog-Materials/raw/master/post_trim.png" width="400">

To test the robustness of digitization quality I used the global analysis tools in the free trial version of Wavelab.  After some hiccups early in the process I have been pleased to find that my tool is performing reliably with no dropped samples! (Right now it is using a pretty large buffer, and one of my next steps will be to test how far I can roll that back).

Overall this has been a very fun process, and has accomplished the goal of ‘Professional Development’ in several ways.  I have been able to gain more experience both with scripting and with manipulating media streams.  Since the project is based on the AMIA Open Source Github site, I have been able to learn more about managing collaborative projects in an open-source context.  While initially intimidating, it has been exciting to work on a tool in public and benefit from constant feedback and support.  Plus, at the end of the day I am left with a free and open tool that fulfills my audio archiving needs!

Audiorecorder is based here https://github.com/amiaopensource/audiorecorder, and is installable via homebrew.  Some of the next steps I am working on are to get a bit of real usage documentation, and to ensure that it is also possible to install audiorecorder via linuxbrew.

I would like to give particular thanks to the contributors who have helped with the project so far, Reto Kromer, Matt Boyd and Dave Rice!
