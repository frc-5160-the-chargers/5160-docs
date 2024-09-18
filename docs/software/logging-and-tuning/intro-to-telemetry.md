# Intro to Telemetry

Everyone hopes that their robot code will work first try, but usually, that's not the case.
Rather, something goes wrong, and it has to be fixed.

The first thing you might have thought is to use the classic print statement. However,
this quickly becomes a bad idea if you have a bunch of stuff you need to debug. Most teams
use a combination of 2 tools: A logging library to log data from their robot code,
and a log viewer app(on your computer) to view those logs.

Our logging is based off of the [DogLog](https://doglog.dev/) logging library
and the [AdvantageScope](https://github.com/Mechanical-Advantage/AdvantageScope) log viewer.

To learn more about robot-side logging, visit our **Logging in Code** section;
for AdvantageScope, read the docs linked above(as we don't use any custom wrappers).