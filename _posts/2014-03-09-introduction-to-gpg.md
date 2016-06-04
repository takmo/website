---
title: An Introduction To GPG
layout: post
---

Recently, I developed an interest in email encryption and began investigating
the technologies needed to use it. I soon found myself looking at GPG, wondering
how it could be used. Lucky for me, someone had written an incredibly useful
tutorial about the subject.

[Creating the perfect GPG keypair - Alex Cabal]
(https://alexcabal.com/creating-the-perfect-gpg-keypair/)

The tutorial itself did an excellent job of getting me set up. However, as a
newbie, I found myself looking back at the commands I had typed and wondering
what I had actually just completed.

In this post, I would like to offer some companion information to compliment the
awesome tutorial written by Alex Cabal. I will be going over a few of the topics
I had difficulty with while learning about GPG.

I am by no means an expert in this, but I hope that this post will make
understanding GPG at least a tad bit easier.

If you have any corrections or criticisms to offer, please feel free to contact
me and I will be happy to amend my post.

## What is GPG? ##

Well, I suppose this is a bit of a loaded question.

When speaking of email encryption, people often toss around two acronyms that
appear ridiculously similar to each other: PGP and GPG.

PGP stands for "Pretty Good Privacy" and is almost always used as a reference to
OpenPGP. To quote from their website, "OpenPGP is the most widely used email
encryption standard in the world." Put simply, OpenPGP is a standard that
explains a method for encrypting emails (and other data.) It does not, however,
actually do any encryption. OpenPGP is just a standard, rules and guidelines for
programs to follow if they want to use OpenPGP style encryption. Given that it
is a standard and not a program, you can't actually download, run, or even
directly use OpenPGP.

That's where GPG comes in. GPG is the GNU Privacy Guard, an *implementation* of
the OpenPGP standard. It is a program that follows all of the guidelines
defined by OpenPGP and actually allows you to encrypt your data.

Put simply: PGP (OpenPGP) is a standard ruleset defining how email encryption
should work. It doesn't do the actual work itself, it just defines the rules. It
is up to the implementations of the ruleset to do actual encryption. GPG (GNU
Privacy Guard) is one of the most popular implementations of OpenPGP that people
use for email encryption.

GPG is the focus of this post.

## So what *is* GPG? ##

GPG is a tool for signing and encrypting emails and other data.

Signing means attaching your digital signature to any email or data you are
sending. When your recipient receives an email with your digital signature on
it, they can be sure that you were the one who sent it, not an imposter.
Additionally, your digital signature will allow your recipient to see if an
evildoer has tampered with your email between the time it leaves your computer
and the time they receive it.

Encrypting your email scrambles your message so that only you and your recipient
can read the message. If anyone else were to look at the message, it would
appear as random indecipherable garbage.

GPG allows you to take advantage of both digitally signing your emails as well
as encrypting them if secrecy if of the utmost importance to you. Even if
you do not feel the need to encrypt your emails, signing them is always a useful
feature.

## How does it work? ##

In order to understand how GPG works, one must first understand how public key
cryptography (asymmetrical cryptography) works. The following link serves as an
excellent guide for understanding public key cryptography. If/when you are
comfortable with the concept, feel free to continue.

[Explaining public-key cryptography to non-geeks]
(https://medium.com/how-to-use-the-internet/f0994b3c2d5)

GPG relies heavily upon this concept. Each user generates a GPG keypair, a
public key and a private key that are used for signatures and encryption.
Following the rules of public key cryptography, anyone can encrypt a message
with your public key and you will be the only one who can read it. Inversely,
you can encrypt a message with your private key and not only can everyone read
it, but they can also be sure that it was actually you who wrote it.

Using this concept, GPG allows for both signing and encryption.

A hash is a digital signature of a piece of data. If anything in the data
changes, even something as small as a single character, the entire hash changes.

When you sign an email, GPG takes the hash of your email, encrypts it with your
private key, and places it in a signature at the end of your message before
sending it off. When your recipient receives the email, they decrypt your
signature with your public key and compare the hash you provided with the hash
of the email they received. If the hashes match up, they are guaranteed that:

1. The email was not tampered with (because that would change the hash.)

2. You were the one who sent the email (because only your private key could have
encrypted it.)

After signing your email, you also have the option to encrypt it with your
recipient's public key. If you choose to do this, you now have an email that:

1. Verifies that the message was not tampered with.

2. Guarantees that *you* sent the message.

3. Guarantees that your recipient is the only one who can read the message.

That's basically what GPG does and how it works.

## A More Detailed Look At Your Key ##

Everything before this was (I hope) fairly straightforward. This is where things
start getting confusing. So far I have been referring to a singular GPG key,
consisting of a public and a private key. In reality, there is a bit more to it
than that.

A single GPG key is actually made up of multiple parts: a *Master Signing Key*,
which represents the key itself, and any number of *subkeys*, each with a specific
purpose. More often than not, a subkey is used for either signing or encrypting.
In addition to acting as the authority in a GPG key, the master signing key is
also used for, of course, signing.

GPG uses different keys for different purposes, usually either signing or
encrypting. All of these keys are grouped together to form the GPG key itself.
To give a better example, here is a sample GPG key.

        gpg --edit testme@bitwisehero.com

        pub  4096R/F398AE55  created: 2014-03-08  expires: never       usage: SC  
                         trust: ultimate      validity: ultimate
        sub  4096R/5A6C5575  created: 2014-03-08  expires: never       usage: E   
        [ultimate] (1). Randall D <testme@bitwisehero.com>

Whenever a GPG key is generated, its structure will look something like this.
The master signing key is marked with "pub" and any subkeys linked to it are
marked with "sub". On the far right is the key usage. The "S" flag stands for
signing and the "E" flag stands for encryption. Other flags exist, but for now
we won't worry about them.

In this case, key F398AE55 is my master signing key. This represents my GPG
key and is also used when I need to sign anything.

My encryption key is 5A6C5575. It is a subkey of my master signing key and is
used whenever I need to send or receive any encrypted data.

To increase the complexity even more, the master signing key and all subkeys
are each made up of both a public key and a private (secret) key, as per the
rules of public key cryptography.

When all of the public keys from my master signing key and all subkeys are
collected together, they are collectively referred to as my Public GPG Key.

All of my keys together, public and private, form my Private GPG Key, or just my
GPG Key.

A GPG key, public or private, will always be represented by its master signing
key ID. In this case, my GPG key ID is F398AE55.

Some GPG keys will consist of more than two keys, but every standard key has an
absolute minimum of these two: exactly one master signing key and at least one
encryption subkey.

## Adding Another Signing Key ##

In the companion tutorial provided at the beginning of the post, the author
follows the standard GPG generation procedure until he decides to add an
additional signing subkey.

As I mentioned earlier, a GPG key consists of a master signing key and any
number of subkeys. In this case, we are generating a new subkey for signing.

        pub  4096R/F398AE55  created: 2014-03-08  expires: never       usage: SC  
                         trust: ultimate      validity: ultimate
        sub  4096R/5A6C5575  created: 2014-03-08  expires: never       usage: E   
        sub  4096R/FA9CF8F9  created: 2014-03-09  expires: never       usage: S   
        [ultimate] (1). Randall D <testme@bitwisehero.com>

The "S" flag on the final subkey states that key FA9CF8F9, a subkey of my
master signing key, is used for signing. A message signed with this new subkey
is just as valid as a message signed with the master signing key.

Now that we have a subkey that we can use for signing instead of the master
signing key, we can increase our security by removing our *private* master
signing key from our working GPG key.

To do this, we make a complete copy of our entire GPG key and store it in a
secure location. This will be our *Master GPG Key*. Then, we simply delete the
private (secret) key part of our master signing key. This becomes our *Working
GPG Key*.

Doing this increases security by making our master (authoritative) signing key
more difficult to steal. If someone were to compromise your machine and gain
access to your Working GPG Key, you could locate your Master GPG Key, revoke
your compromised keys, and generate new subkeys to use. By doing this, the
criminal would be unable to impersonate you and you would be able to maintain
your "identity".

Note: If a criminal gains access to your Working GPG Key, even without the
master signing key, they can still decrypt any data you may have stored.
However, by separating the master signing key from the subkeys, you can now
revoke the stolen subkeys and the criminal is unable to impersonate you.

## What is a...? ##

This last bit covers a few terms that are useful to know.

**Keyserver**: An online database of GPG keys that allows you to easily share
your GPG keys with others while simultaneously allowing you to easily find
someone else's GPG key, if it is also stored in the keyserver. Almost all
keyservers, with a few notable exceptions, are linked together in a pool.
Assuming you choose a keyserver in the pool, it should not matter which specific
keyserver you use.

**Keysigning**: A way of saying that you trust another person's key. Anyone can
generate a key with someone else's email and impersonate them with enough
effort. In order to verify that you are who you say you are, you can ask others
to sign your master signing key using their master signing key. This is a mark
from that person saying, "Yes, this person is who they say they are." Typically,
the more key-signatures a GPG key has, the more trustworthy it is.

**Revocation Certificate**: A special certificate used to revoke your master
signing key in the event that it is compromised.

## Conclusion ##

I suppose this about wraps everything up. As you can see, GPG is a very useful
technology. I hope that this post makes it easier to understand.

