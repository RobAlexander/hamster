# Hamster

This web app provides a virtual pet. It is a hamster, and is rendered in 3d in moderate detail. Periodically it demands food, which you can pick up from a ui element and drop on it. 

* Quotes from the king james bible, the aprocrypha, and the gnostic gospels - go and get a large (say 200k) set of random verses from those and keep it in the file.

* Sometimes it just stops and _looks at you_ for a long time. 

* Sometimes Wanders out if shot and back again.

* You can summon it to the "looking out a you" position with a a double click.

* UI takes your name on first run, so hamster can talk to you. It says your name before every utterance, to get your attention.

* There is a menu or similar with options e.g. name change and food change.

* Sometimes it will demand the substance and then it will sleep for a long time. You can only give it the substance when it asks for it.

* Asks about twice a day for its lotion to be applied. When you do, it makes a weird noise. Like the substance, you can only give when it asks for it.

* Will sometimes become very worried about "a presence" which is in the room with the computer ie where the user is. It will move faster and more jerkily "look around the room" (i.e. out of the screen, but in different angles) and make various concerned growling noises.

* It has some stuff it will reliably say, once, once total time since first run reaches a particular thereshold hold. e.g. "I have been with you for 14 days, Peter, and yet I have not truly seen your heart".

* Very occassionally plays dead, lies rigidly on its back. If you click it, it wobbles.

* thing it can say "I have been reading the news, Peter"

* Sometimes follows the mouse pinter with its eyes, sometimes doesnt.

* All speech is generated as text via a custom markov chain system, creating a wide range of specific variations on the messages I'm suggesting here. with the exeception of religious quotes which are always verbatim. This text is then turned to speech using the easiest and least-security-risk option, probably the browsers own speech synth system. The hamster is always quite serious and formal, and does not swear. 

* It sometimes it poops pellets, which it will ask you to pick up by clicking on the. If you leave them a long time eg an hour they will grow legs and crawl away. Sometimes bugs enter it's area, which it will study for a while, then maybe eat. 

* Sometimes it's tells you that it's got a gun, or similar e.g. it has set traps in case of an intruder, it has contacts in the police force.

* Please implement as a single-file Javascript thing, to run in any browser. Use localstorage for persistence. You can use external libraries from standard repositories but don't use any external apis or other sources at runtime.

* Note in claude.md that we might eventually let it "read the news" by pulling headlines etc from selected websites e.g. bbc news, bbc weather, and saying things "peter, i am worried about the iran situation"

* Please start by making a phased plan to implement this, and writing the claude.md (including overview of phases)