<img src="https://avatars1.githubusercontent.com/u/20255067?s=200&v=4" align="right">

**Table of contents**\
  [Preamble](#preamble)
| [Overview](#overview)
| [Mindset](#mindset)
| [Reporting vulnerabilities](#reporting)
| [Best practices](#bcp)

<a name="preamble"></a>
## [¶](#top) Preamble

For the purposes of this document, we define [security] as the need to make sure we properly authenticate who is accessing the
data and that they have the correct permissions to do so. We primarily focus on _before the event_ scenarios intended to reduce
the risk of security alerts. The GitHub-related workflows targeted by this document have limited access to historical or archival
data or logs, required by _after the event_ security tracking to show evidence when something has gone wrong.

  [security]: https://dictionary.cambridge.org/dictionary/english/security

The key words _MUST_, _MUST NOT_, _REQUIRED_, _SHALL_, _SHALL NOT_, _SHOULD_, _SHOULD NOT_, _RECOMMENDED_, _NOT RECOMMENDED_,
_MAY_, and _OPTIONAL_ in this document are to be interpreted as per [BCP 14][bcp14] complemented by [RFC 8174][rfc8174]).

  [bcp14]: https://tools.ietf.org/html/bcp14
  [rfc8174]: https://tools.ietf.org/html/rfc8174

<a name="overview"></a>
## [¶](#top) Overview

>   The community benefits of GitHub are substantial, but they also carry potential risks. The fact that anyone can propose bug
>   fixes publicly comes with certain responsibilities. The most important is the responsible disclosure of information that
>   could lead to security exploits before their underlying bugs can be fixed. Developers looking to report or address security
>   issues look for a `SECURITY.md` file in the root of a repository in order to responsibly disclose their concerns. Providing
>   guidance in this file will ultimately speed up the resolution of these critical issues.

This document introduces a secured development strategy for all [open][open source] and [inner source] projects whose source code
is maintained on the [ISLEcode] GitHub account. It delves into important considerations repository maintainers and contributors
should account for outside of the core development (i.e. code writing) process.

  [inner source]:   https://en.wikipedia.org/wiki/Inner_source
  [open source]:    https://en.wikipedia.org/wiki/Open_source
  [islecode]:       https://github.com/ISLEcode

This strategy is organised as a set of recommendations and best practices for GitHub-based software development workflows, to
ensure that information isn't inappropriately altered or destroyed, and to protect confidential information from being disclosed
to people that should not have access to it.

<a name="mindset"></a>
## [¶](#top) Mindset

Building and deploying secure software components and applications involves many aspects; keep the following considerations in
mind at all times:

-   **I know that I know nothing about SECURITY**\
    Cybersecurity is a constantly evolving discipline; adopt the [Socratic paradox][socratic-paradox].

-   **Keep it simple, SECURE**\
    Revisited [KISS principle][kiss-principle] that insuflates security as a primary concernt from day zero.

-   **SECURED day-to-day delivery processes**\
    Recurring security testing through [DevOps] workflows enforcing the organisations' rules and regulations.

  [socratic-paradox]:   https://en.wikipedia.org/wiki/I_know_that_I_know_nothing
  [kiss-principle]:     https://en.wikipedia.org/wiki/KISS_principle
  [devops]:             https://en.wikipedia.org/wiki/DevOps

<a name="reporting"></a>
## [¶](#top) Reporting vulnerabilities

Please email reports about any security related issues you find to ISLE's [Security Office](mailto:secmon@isle.plus) Your email
will be acknowledged within one business day, and you'll receive a more detailed response to your email within 7 days indicating
the next steps in handling your report.

**Important**: Report security bugs in third-party modules to the person or team maintaining that module.

Please use a descriptive subject line for your report email prefixed with the keyword `SECMON`. After the initial reply to your
report, the security team will endeavor to keep you informed of the progress being made towards a fix and announcement.

In addition, please include the following information along with your report:

-   Your name and affiliation (if any).

-   A description of the technical details of the vulnerabilities.

    It is very important to let us know how we can reproduce your findings.

-   An explanation who can exploit this vulnerability, and what they gain when doing so — write an attack scenario.
    This will help us evaluate your report quickly, especially if the issue is complex.

-   Whether this vulnerability is public or known to third parties. If it is, please provide details.

    If you believe that an existing (public) issue is security-related, the email should include the issue ID and a short
    description of why it should be handled according to this security policy.

Once a vulnerability reported, [ISLEcode] uses the following disclosure process:

-   When a report is received, we confirm the issue and determine its severity.

-   If we know of specific third-party services or software based on ISLEcode that require mitigation before publication, those
    projects will be notified.

-   An advisory is prepared (but not published) which details the problem and steps for mitigation.

-   Wherever possible, fixes are prepared for the last minor release of the two latest major releases, as well as the master
    branch. We will attempt to commit these fixes as soon as possible, and as close together as possible.

-   Patch releases are published for all fixed released versions, and the advisory is published.

Past security advisories are kept in the `.github/SECURITY` folder of the related repository.
See the sample [README.md][saread] and [report][sareport] files.

  [saread]:     https://github.com/ISLEcode/.github/blob/master/.github/SECURITY/README.md
  [sareport]:   https://github.com/ISLEcode/.github/blob/master/.github/SECURITY/sa-yyyy-xxx.md

_Note_: We credit reporters for identifying security issues, although we keep your name confidential if you request it.

<a name="bcp"></a>
## [¶](#top) Best practices

1.  **Don't keep ANY sensitive files in a repository**

    > It should be assumed that any data committed to GitHub at any point has been compromised.

    _Sensitive files_ are not always easily detectable, these could be – and often are, benign intermediate build files typically
    used for testing purposes and which contain, for instance, API keys or private configuration data.

    1.  Before the event security is commonly achieved through:

        -   Exhaustive enumeration of files which SHOULD NEVER be commited in [.gitignore][] files. These files instruct client
            tools, such as the `git` command line utility, to [ignore paths and patterns][.gitignore-eg] when aggregating files
            for a commit. Though multiple such files can be created in a same Git repository; we recommend concentrating all rules
            in a single file in the top level directory.

        -   Always clean up your sandbox environment before commiting changes. For instance, when using the GNU Autotools build
            toolchain, perform a `make distclean` – and not `make clean` or `make realclean`, before any commit.

        -   The `.gitignore` method is not bullet proof. Repository maintainers should, at a recurring interval — which should at
            least be twice a year, check all files in the repository which are under revision control looking for those which may
            contain sensitive data.

    1.  After the event security occurs when sensitive information has been commited. The Git database needs to be cleanup to
        remove all traces of that information; simply overwriting a commit isn't enough to ensure the data will not be accessible
        in the future. See GitHub's instructions on [removing sensitive data from a repository][zap-git-db].

  [.gitignore]:     https://help.github.com/github/using-git/ignoring-files
  [.gitignore-eg]:  https://github.com/github/gitignore
  [zap-git-db]:     https://help.github.com/github/authenticating-to-github/removing-sensitive-data-from-a-repository

1.  **TRACK your dependencies**

    Most projects these days take dependencies on external packages, which in turn have their own dependencies, etc. This brings
    complexity and additional security risk; repository maintainers, often contributing to projects on a _best effort_ basis will
    hardly stay on top of these packages and vulnerability status.

    1.  An essential requirement to mitigate such security vulnerabilities is to regularly update all packages to their latest
        release. Today, most programming languages and operating platforms have at least one package manager; such package
        managers allow to conveniently update such packages — e.g. `yum update` for RHEL and CentOS, `composer update` for PHP,
        or `npm update` for NodeJS. Such updates should be included in the build workflow.

    2.  A list of the top-level packages required to build the software component or application should be maintained as a human
        readable and managed file. The provides a quick reference for repository maintainers and contributors to track required
        packages, the version required by the project, and possible security annotations.

    3.  GitHub provides [dependency tracking][gh-dtrack] insights and the [Dependabot][gh-depbot] utility for selected packages
        ecosystems; two of which are on ISLE's technology roadmap: namely PHP's composer and NodeJS' npm. These should be
        configured to automate dependency alerts and create pull requests to update the project.


  [gh-dtrack]: https://bit.ly/2Ezoyhq
  [gh-depbot]: https://bit.ly/31qxyya

