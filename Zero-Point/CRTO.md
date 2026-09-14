# CRTO: Learning to Think Like an Operator

<p align="center">
  <img src="assets/crto.png" alt="Zero-Point Security Red Team Ops" width="200">
</p>

## Introduction

Back in August, I completed the **Certified Red Team Operator (CRTO)** certification from Zero-Point Security. I've had some time to look back on the course and exam, so I wanted to share what I took away from it.

I currently work as a **Network Security Administrator**, but offensive security is where I want to take my career, with red teaming being the ultimate goal.

Going into CRTO, I already had experience with penetration testing, Active Directory, Kerberos, and other offensive security training. I had also completed CRTP, so a lot of the underlying AD concepts weren't completely new to me.

CRTO still made me think differently.

The biggest change was learning to stop asking:

> **"What attack can I run?"**

and start asking:

> **"What does the information in front of me actually allow me to do?"**

That's probably the biggest lesson I took away from the course.

I'm not going to cover exam objectives, flags, or solutions here. This is more about how CRTO changed the way I approach an environment and what I'd recommend to someone preparing for it.

---

## Don't Memorize the Attack

If I had to give someone one piece of advice before CRTO, it would be:

> **Don't memorize the attack. Understand the relationship you're abusing.**

It's really easy to make notes like this:

```text
Attack X

Command 1
Command 2
Command 3

Profit 😎
```

I've made plenty of notes like that.

They're great when you're doing a lab that basically tells you what vulnerability you're supposed to exploit. They're a lot less useful when nobody tells you what you're looking for.

Instead, I started asking myself simpler questions:

- Who am I?
- What privileges do I have?
- What machine am I on?
- What credentials or tickets do I have?
- Where can this account authenticate?
- What relationships have I found?
- Does anything look unusual?
- Why does it matter?

That last question is huge.

If enumeration gives you an attribute or relationship you've never seen before, don't ignore it because it isn't immediately useful. Look it up. Figure out what it controls and why it's configured that way.

Sometimes that weird line of output is exactly what you've been looking for.

---

## Keep Enumerating

One mistake that's easy to make is treating enumeration as something you only do at the beginning.

Enumerate the domain, find something interesting, attack it, move on.

The problem is that your position keeps changing.

If you compromise another user, that user may have completely different access.

If you land on another machine, there may be different sessions, credentials, services, and configurations.

If your privileges change, you may be able to see things you couldn't before.

So whenever my position changed, I tried to remind myself to look around again.

```text
Enumerate
    ↓
Find something
    ↓
Understand it
    ↓
Test it
    ↓
Gain new access
    ↓
Enumerate again 🔄
```

I also stopped immediately running through a mental checklist every time I got access to something:

```text
Kerberoasting?
AS-REP roasting?
Delegation?
AD CS?
Credential dumping?
BloodHound?
```

There's nothing wrong with knowing those techniques. The problem is using them just because you know them.

I had much better results when I let the environment tell me what to look at next.

Find something interesting. Understand what it means. Figure out whether you can do anything with it. Then choose the technique.

It sounds obvious, but under pressure it's really easy to start throwing commands at an environment hoping something sticks.

---

## Learn Kerberos

If you're preparing for CRTO, spend some time understanding Kerberos.

Seriously.

You don't need to know every detail of the protocol, but this should make sense:

```text
User
 ↓
TGT
 ↓
TGS
 ↓
Service
```

Understand what a TGT is, what a service ticket is, why SPNs matter, and how those tickets are used to authenticate to services.

Once I understood what was happening underneath the attacks, concepts like delegation made a lot more sense.

That helped me more than memorizing individual Kerberos techniques.

If Kerberos still feels like mysterious Windows magic, spend some extra time there before the exam.

It'll pay off.

---

## Learn Your C2

Another big takeaway for me was learning not to treat a C2 like a fancy remote command prompt.

Take some time to actually understand how it works.

Know the basics around:

- Beacons
- Sleep and jitter
- Jobs
- Pivoting
- SOCKS proxies
- Credential and ticket handling
- BOFs
- .NET execution
- Process execution

More importantly, start paying attention to what happens when you run something.

Does it spawn another process?

Does it write something to disk?

Is there a built-in capability that can accomplish the same thing?

How much unnecessary activity are you creating?

These weren't always things I thought about when I first started doing offensive security labs. If the command worked and I got what I needed, I was happy.

As I've gotten more interested in red teaming, I've started thinking about that side of things a lot more.

---

## "Can I?" vs. "Should I?"

This is probably one of the bigger mindset changes I'm still working on.

In a lab, the question is usually:

> **"Can I do this?"**

I'm trying to get better at adding another question:

> **"Should I do this?"**

Just because I have a tool or technique available doesn't mean I immediately need to use it.

Does it actually help me?

Am I creating a process I don't need?

Am I touching disk?

Am I generating unnecessary traffic?

Would a built-in capability accomplish the same thing?

What would this look like from the defender's side?

I'm still learning this part. Passing CRTO obviously didn't turn me into a red team operator overnight.

It did get me thinking more about the difference between simply achieving an objective and how you actually got there.

That's something I want to keep improving on.

---

## When You Get Stuck

You're going to get stuck.

I definitely did.

When it happens, throwing more attacks at the problem usually doesn't help.

Go back through what you actually know:

```text
Where am I?
Who am I?
What privileges do I have?
What credentials or tickets do I have?
What can I reach?
What have I found so far?
What changed since I last looked around?
```

I also found it useful to separate two questions:

> **Is my idea wrong?**

from:

> **Did I just execute it wrong?**

There's a big difference.

You can have the correct attack path and screw up the syntax. You can also have a perfectly valid command for an attack that makes absolutely no sense in your current situation.

And sometimes you've just been staring at the terminal for too long and there's a typo right in front of you 😂

Don't let the clock convince you that going faster is always the answer.

---

## Take Notes That Actually Help You

I'd avoid turning your notes into one giant command cheat sheet.

For each technique, document things like:

```text
What makes this possible?
How do I find it?
What access do I need?
What does it give me?
What are the limitations?
What does successful output look like?
What should I look at next?
```

Then add your commands.

That way, when you find something interesting, your notes help you determine whether a technique actually applies instead of just giving you something to copy and paste.

Future you will appreciate it too.

---

## What I'd Focus on Before CRTO

If I were preparing again, I'd keep it pretty simple:

1. **Know your Active Directory fundamentals.**
2. **Learn Kerberos. Seriously.**
3. **Understand why attacks work instead of memorizing commands.**
4. **Get comfortable with your C2 before the exam.**
5. **Build notes around prerequisites and relationships.**
6. **Enumerate again whenever your access changes.**
7. **Actually read your output.**
8. **When you're stuck, verify what you know before looking for something more complicated.**
9. **Get comfortable researching things you don't recognize.**
10. **Stay curious.**

That last one helped me more than I expected.

If you find something you don't recognize, look into it. Don't assume it isn't relevant just because you haven't seen it before.

---

## CRTO Didn't Make Me a Red Teamer

Passing CRTO obviously didn't make me a red team operator overnight.

If anything, it showed me how much more there is to learn about C2, OPSEC, Windows internals, EDR, identity attacks, payload development, and the infrastructure behind an operation.

That's also what keeps me interested in this field.

I'm not interested in collecting enough certifications to suddenly call myself a red teamer. I want to build the knowledge and experience to eventually become a good one.

CRTO was another step in that direction, not the finish line.

---

## Final Thoughts

The biggest thing I've carried with me from CRTO is a different way of approaching problems.

Look at what you have.

Understand what it means.

Figure out what relationships exist.

Form an idea.

Test it.

See what changed.

Then look around again.

Tools and commands are important, but they're also the easiest part to look up.

Understanding **why** something works is harder, and I think it's a lot more valuable.

I'm still early in my path toward red teaming and have plenty left to learn, but CRTO helped point me in the direction I want to go.

If I had to sum up the entire experience in one line:

> **Don't memorize the attack. Understand the relationship you're abusing.**

That's the lesson I'm taking with me into whatever comes next. 🤙
