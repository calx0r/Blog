# CRTO: Learning to Think Like an Operator

## Introduction

Back in August, I completed the **Certified Red Team Operator (CRTO)** certification from Zero-Point Security. I've had a few weeks to look back on the course and exam, so I wanted to share my experience and some advice for anyone thinking about taking it.

I currently work as a **Network Security Administrator**, but my goal is to transition into offensive security and eventually work as a red team operator. That's a big reason I decided to take CRTO in the first place.

Going into it, I already had experience with penetration testing, Active Directory, Kerberos, and other offensive security training. I had also completed CRTP, so a lot of the underlying AD concepts weren't completely new to me.

CRTO still made me think differently.

The biggest thing I took away from it was learning to stop asking:

> **"What attack can I run?"**

and start asking:

> **"What does the information in front of me actually allow me to do?"**

That's what I want to focus on here.

I'm not going to cover exam objectives, flags, or solutions. There are plenty of reviews that talk about the course material already. I want this to be more about how I approached CRTO, how I would prepare for it, and what I learned from the experience.

---

## What Is CRTO?

**CRTO (Certified Red Team Operator)** is the certification for Zero-Point Security's **Red Team Ops** course.

It's focused heavily on Windows and Active Directory environments and teaches you how to work through them using a command-and-control framework.

Some of the main areas you'll run into are:

- Active Directory enumeration
    
- Kerberos
    
- Credential access
    
- Privilege escalation
    
- Lateral movement
    
- Delegation
    
- Persistence
    
- Pivoting
    
- Command and control
    

The individual techniques are useful, but I don't think that's the main value of the course.

You can learn how to Kerberoast an account or abuse a specific AD configuration from a blog post. The harder part is recognizing **when that technique actually applies** and how one piece of information connects to another.

You might compromise a system and find an interesting account. That account gives you access somewhere else. On that system, you find another credential or an interesting AD relationship.

Eventually, those small discoveries start forming a path.

That's where CRTO gets interesting.

---

## Who Is CRTO For?

I wouldn't make CRTO your first offensive security course.

You don't need to already work as a pentester or red teamer. I don't. But having some experience with Windows and Active Directory will make the course a lot easier to digest.

Before starting, I'd recommend being comfortable with:

- Windows and Active Directory
    
- Basic networking
    
- PowerShell
    
- NTLM and Kerberos
    
- Windows authentication
    
- Basic privilege escalation
    
- Basic lateral movement
    
- Reading command-line output
    

You don't need to be an expert in all of those.

You should at least understand them well enough that you're learning the attack techniques instead of trying to learn Windows, networking, Active Directory, and offensive security all at the same time.

Being comfortable researching things on your own helps a lot too.

---

## My Background Going Into CRTO

Most of my professional experience is on the infrastructure and defensive side of security.

As a Network Security Administrator, I work with Active Directory, networking, identity, endpoint security, virtualization, Microsoft security tooling, and other enterprise infrastructure.

Offensive security is what I've been pursuing outside of work.

Before CRTO, I had already spent a lot of time working through penetration testing and Active Directory material. I had also completed **CRTP**, so concepts like Kerberos tickets, SPNs, delegation, lateral movement, and AD permissions were familiar.

That definitely gave me a head start.

At the same time, CRTO showed me that knowing how an attack works doesn't necessarily mean you'll recognize when you can use it.

I think that's an important distinction.

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

They're great when you're doing a lab that basically tells you what vulnerability you're supposed to exploit.

They're a lot less useful when nobody tells you what attack you're looking for.

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

If LDAP enumeration gives you an attribute you've never seen before, don't ignore it because it isn't immediately useful. Look it up. Figure out what it controls and why it's configured that way.

Sometimes that one weird line of output is exactly what you've been looking for.

---

## Learn Kerberos

I would spend a decent amount of time on Kerberos before taking CRTO.

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

You should understand what a TGT is, what a service ticket is, why SPNs matter, and how those tickets are used to authenticate to services.

From there, concepts like delegation start making a lot more sense.

This helped me more than memorizing individual Kerberos attacks. Once I understood what was happening underneath them, I had a much easier time figuring out why a particular technique might work.

If Kerberos still feels like some mysterious Windows magic, spend some extra time there before the exam.

It'll pay off.

---

## Keep Enumerating

One mistake that's easy to make is treating enumeration as something you do at the beginning.

Enumerate the domain, find something interesting, attack it, move on.

In reality, your position keeps changing.

If you compromise another user, that user may have completely different access.

If you land on another machine, there may be different sessions, credentials, services, and configurations.

If you become SYSTEM, you can probably see things you couldn't before.

So whenever my position changed, I tried to remind myself to look around again.

```text
Enumerate
    ↓
Find something
    ↓
Test it
    ↓
Gain new access
    ↓
Enumerate again 🔄
```

It's simple, but it's easy to forget when you're focused on getting to the next objective.

---

## Let the Environment Tell You What to Do

When I first started learning AD attacks, it was easy to get a foothold and immediately start running through a mental checklist:

```text
Kerberoasting?
AS-REP roasting?
Delegation?
AD CS?
Credential dumping?
BloodHound?
```

There's nothing wrong with knowing those techniques.

The problem is running them just because you know them.

I had much better results when I slowed down and let the information I collected determine what I looked at next.

Find something interesting.

Understand what it means.

Figure out whether you can do anything with it.

Then choose the technique.

It sounds obvious written out like that, but under exam pressure it's really easy to start throwing commands at the environment hoping something sticks.

---

## Learn Your C2

Don't treat your C2 like a remote command prompt.

Take some time to understand how it works.

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
    

I'd also pay attention to what happens when you run something.

Does it spawn another process?

Does it write something to disk?

Is there a built-in capability that can do the same thing?

How much traffic are you generating?

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

Am I generating a bunch of unnecessary traffic?

Would a built-in C2 capability accomplish the same thing?

What would this look like from the defender's side?

I'm still learning this part. Passing CRTO obviously didn't turn me into a red team operator overnight.

It did get me thinking more about the difference between simply getting an objective and how you actually got there.

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

And sometimes you've been staring at the terminal for too long and there's a typo right in front of you 😂

Don't let the clock convince you that going faster is always the answer.

---

## Take Notes That Actually Help You

Notes are obviously important for an exam like CRTO.

I'd just avoid turning them into one giant command cheat sheet.

For each technique, try to document things like:

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

That way, when you find something interesting, your notes help you understand whether the technique applies instead of just giving you something to copy and paste.

Future you will appreciate it too.

---

## My Advice for Preparing

If I were preparing for CRTO again, I'd keep it pretty simple:

1. **Know your Active Directory fundamentals.**
    
2. **Learn Kerberos. Seriously.**
    
3. **Understand why attacks work instead of memorizing commands.**
    
4. **Get comfortable with the C2 before the exam.**
    
5. **Build notes around prerequisites and relationships.**
    
6. **Enumerate again whenever your access changes.**
    
7. **Read your output instead of blindly moving to the next command.**
    
8. **When you're stuck, verify what you know before looking for something more complicated.**
    
9. **Get comfortable researching unfamiliar things.**
    
10. **Stay curious.**
    

I think that last one helped me more than I expected.

If you find something you don't recognize, look into it. Don't assume it isn't relevant just because you haven't seen it before.

---

## CRTO Didn't Make Me a Red Teamer

I want to be clear about this because of the name of the certification.

I passed the **Certified Red Team Operator** exam.

I don't consider myself a red team operator because of that.

I'm a Network Security Administrator who's working toward transitioning into offensive security and eventually red teaming.

CRTO is another step in that direction.

There's still a ridiculous amount I want to learn about Windows internals, EDR, C2 infrastructure, OPSEC, payload development, identity attacks, detection, and red-team infrastructure.

The more I learn, the more I realize how much I don't know.

That's part of what keeps this interesting.

I'm not trying to collect enough certifications to suddenly declare myself a red teamer. I want to build the knowledge and experience to eventually become a good one.

---

## Final Thoughts

A few weeks after finishing CRTO, I think the biggest thing I've carried with me is a different way of approaching problems.

Look at what you have.

Understand what it means.

Figure out what relationships exist.

Form an idea.

Test it.

See what changed.

Then look around again.

Tools and commands are obviously important, but they're the part that's easiest to look up.

Understanding **why** something works is harder, and I think it's a lot more valuable.

I'm still early in my path toward red teaming and have plenty left to learn. CRTO didn't make me an operator, but I do think it helped point me in the right direction.

If I had to sum up the entire experience in one line:

> **Don't memorize the attack. Understand the relationship you're abusing.**

That's the lesson I'm taking with me into whatever comes next. 🤙
