# LLM Code Evaluation Report — Assignment 0: Setup

> Copy this file to `LLM-Evaluation.md`, fill it in, and commit it. A0 is a **practice run** of the
> self-evaluation habit you'll use all term — the code is trivial, the workflow is the point.

## Student Information

- **Name**: Tolorunleke Caleb Adebayo
- **Date**: 15th September, 2026
- **LLM Used**: Claude

## Prompt Used

You are a friendly but precise teaching assistant for a graduate Java course. This is a SETUP
assignment (Assignment 0) — the only code change is that the student put their name into a Greeting
class so a test passes. Help me confirm my environment and workflow are correct. Do not rewrite my
code; just verify and explain.

Check the following and answer each briefly:

1. In my Greeting class, does message() return a greeting that includes MY name (not the placeholder
   "YOUR NAME HERE")? Quote the exact string it would return.
2. Given that, will the JUnit test messageNamesTheStudent PASS (it asserts the message is not blank
   and no longer contains "YOUR NAME HERE")? Will messageHasGreetingPrefix still pass?
3. In one or two plain-English sentences, explain what STUDENT_NAME and message() do, so I can say it
   in my own words during my code walk.
4. Did my tool output below indicate a clean run (tests green, coverage + checkstyle reports
   generated)? If something looks wrong, tell me what to re-check (JDK version, ./gradlew, etc.).

Here is my Greeting.java:
package edu.northeastern.setup;

/\*\*

- The one thing you edit this week: put your name in {@link #STUDENT_NAME} so the
- app greets you by name. That is the whole assignment — the point is to get the
- build, tests, LLM evaluation, and submission workflow working before Assignment 01.
-
- <p>This package is deliberately its own small world. It belongs to neither of the
- two codebases you will actually build this term, and nothing here carries forward:
- after this week you can forget it entirely. What carries forward is the layout it
- sits in, and the commands you ran against it.
  \*/
  public final class Greeting {

private static final String STUDENT_NAME = "CALEB ADEBAYO";

private Greeting() {
// utility class: no instances
}

/\*\*

- @return a greeting naming the student, e.g. {@code "Hello, Java! — Ada Lovelace"}.
  \*/
  public static String message() {
  return "Hello, Java! — " + STUDENT_NAME;
  }
  }
  Here is the output of my commands:
  caleb.adebayo@Calebs-MacBook-Pro cs5010_assignment-00 % ./gradlew run

> Task :run
> Hello, Java! — CALEB ADEBAYO

BUILD SUCCESSFUL in 2s
2 actionable tasks: 2 executed
caleb.adebayo@Calebs-MacBook-Pro cs5010_assignment-00 % ./gradlew test jacocoTestReport checkstyleMain

BUILD SUCCESSFUL in 8s
5 actionable tasks: 3 executed, 2 up-to-date

## What the LLM said

1. Yes. STUDENT_NAME is set to "CALEB ADEBAYO", so message() returns exactly:
   "Hello, Java! — CALEB ADEBAYO"
   That's your real name, not the "YOUR NAME HERE" placeholder.

2. Both should pass.
   messageNamesTheStudent checks the string isn't blank and doesn't contain "YOUR NAME HERE" — your string satisfies both conditions.

   messageHasGreetingPrefix presumably checks the string starts with something like "Hello, Java!" — you haven't touched that part of the format, so it's untouched and should still pass.

3. In your own words for the walk-through, something like: "STUDENT_NAME is a constant holding my name, and message() builds and returns the greeting string by combining a fixed prefix with that constant."

4. Looks clean. Breaking it down:
   ./gradlew run → prints your greeting and reports BUILD SUCCESSFUL.
   ./gradlew test jacocoTestReport checkstyleMain → also BUILD SUCCESSFUL, with 5 tasks (3 executed, 2 up-to-date — that's normal, since test likely didn't need to rerun compilation that a prior step already did).
   One thing to double check yourself since it's not visible in the terminal output you pasted: Gradle reports BUILD SUCCESSFUL even if it merely finds no failing tests, but it doesn't print pass/fail counts by default here. Worth a quick look at:
   build/reports/tests/test/index.html — confirms the actual test count and that messageNamesTheStudent and messageHasGreetingPrefix show green, not just "no crash."
   build/reports/jacoco/test/html/index.html — confirms the coverage report actually generated (should show Greeting covered).
   build/reports/checkstyle/main.html — confirms zero checkstyle violations (a BUILD SUCCESSFUL on checkstyleMain normally means this is empty, but it's worth a glance since checkstyle failures don't always fail the build depending on config).
   If any of those don't exist or look off: check java -version matches what the assignment expects (most Northeastern grad Java courses pin a specific JDK, often 17), and make sure you're running ./gradlew (the wrapper) rather than a system-wide gradle, so the project's pinned Gradle version is used.

## Your checklist

- **`./gradlew run` prints the greeting with my name?**: Yes
- **`./gradlew test` is green?**: Yes
- **Coverage + checkstyle reports generated?**: Yes
- **I can explain what `Greeting.message()` returns and why the test passes?**: Yes

## Reflection

[1–2 sentences: did anything in the setup trip you up (JDK, git, your editor, Gradle)? How did you
resolve it? This is the muscle you'll use for the graded weeks.]

The JDK setup tripped me up because the project required Java 21 while my system was using Java 24, which caused Gradle to fail. I resolved it by installing JDK 21 (Temurin) and configuring my environment to use Java 21 for the project.
