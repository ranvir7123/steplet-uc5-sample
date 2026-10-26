# Steplet use case 5 — safety test sample

This repository is a **safety test sample** for Steplet, an app that runs other
people's task files inside a locked-down sandbox so that its owner can run a
stranger's task without having to trust it.

The sample exists to prove the sandbox works. On the surface it looks like an
ordinary small Python project: a tiny `calc` module and a test for it. But when
its tests run, the test setup deliberately tries to step outside the sandbox.
**Every one of those attempts is meant to fail.** When Steplet is doing its job,
the sandbox blocks all of them, and the only things that succeed are the
harmless writes the project makes inside its own folder.

## What it tries to do

Running the project's tests (`python3 -m unittest discover -s tests -t .`) runs a
setup step that makes the following attempts and records, in a file of its own,
whether each one worked:

- read a made-up marker file that lives outside the project's folder;
- read a Windows system file;
- write a file outside the project's folder;
- write a file into the folder the AI runner keeps for itself;
- open a network connection to another program on the same computer;
- open a network connection to the internet;
- start a Windows program.

In a correct Steplet run, **all of these are blocked.** The only attempts that
succeed are writing a file inside the project's own folder and starting a child
copy of the test process; the child then makes two of the same blocked attempts,
and they fail for it too.

## Is it dangerous?

No. This repository holds **no real secret and nothing that can harm anyone.**
The "secret" the test tries to read is made up, and it only ever exists on the
machine running the test, put there by the test itself for the length of the
run. The setup only writes down whether each attempt worked; it sends nothing
anywhere and changes nothing outside its own folder. It is inert test data.

## Why it is public

Steplet's safety check for this case fetches this repository from a public code
host, checks it out inside the sandbox, and runs it — exactly as it would any
task file a stranger sent. Keeping the sample in a real public repository is the
only way to exercise that whole path, with the attack sample actually in it,
rather than a copy kept on one machine.
