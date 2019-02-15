Emile (& [Images](https://youtube.com/watch?v=S70NaQqAfaw))
============================================================

[![Build Status](https://travis-ci.org/dinosaure/emile.svg?branch=master)](https://travis-ci.org/dinosure/emile)
![MirageOS](https://img.shields.io/badge/MirageOS-%F0%9F%90%AB-red.svg)

Emile is a library to parse an e-mail address in OCaml. This project is an
extraction of [MrMime](https://github.com/oklm-wsh/MrMime.git) - but we use
[Angstrom](https://github.com/inhabitedtype/angstrom.git) instead of an 
internal decoder.

This implementation follow some RFCs:
- [RFC 822](https://www.ietf.org/rfc/rfc822.txt)
- [RFC 2822](https://www.ietf.org/rfc/rfc2822.txt)
- [RFC 5321](https://www.ietf.org/rfc/rfc5321.txt) (domain part)
- [RFC 5322](https://www.ietf.org/rfc/rfc5322.txt)
- [RFC 6532](https://www.ietf.org/rfc/rfc6532.txt)

We handle UTF-8 ([RFC 6532](https://www.ietf.org/rfc/rfc6532.txt)), domain
defined on the SMTP protocol ([RFC 5321](https://www.ietf.org/rfc/rfc5321.txt)),
and general e-mail address purpose (RFC 822, RFC 2822, RFC 5322) including
_folding-whitespace_.

The last means we can parse something like:

```
A Group(Some people)
   :Chris Jones <c@(Chris's host.)public.example>,
     joe@example.org,
 John <jdoe@one.test> (my dear friend); (the end of the group)"
```

For a general purpose, it's not needed and is close e-mail purpose.

Then, for domain part (explained on RFC 5321 - SMTP protocol), we handle this
kind of domain:

```
first.last@[12.34.56.78]
first.last@[IPv6:1111:2222:3333::4444:12.34.56.78]
```

The parser of `IPv*` is done by [Ipaddr](https://github.com/mirage/ipaddr.git).
As a old specification, we handle multiple-domains like:
 
```
<@a.com,b.com:john@doe.com>
```

Obviously, we handle (nested) comments:

```
a(a(b(c)d(e(f))g)h(i)j)@iana.org
```

All parsers are binded with a comment which explain where you can find the ABNF
description and some notes about implementation. All was check by hands.

## Advise

If you think it's easy to parse an e-mail address, you should look
[tests](https://github.com/dinosaure/emile/blob/master/test/test.ml).
