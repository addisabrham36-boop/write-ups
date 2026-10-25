# SDLC — TryHackMe

Room: https://tryhackme.com/room/sdlc

SDLC means Software Development Life Cycle. It describes the stages a team uses to plan, build, test, release, and maintain software. Security works best when it is included throughout development rather than added after a vulnerability becomes an incident.

## Common stages

- **Planning and requirements:** define what the software should do and the risks it must handle.
- **Design:** decide how components interact and where trust boundaries exist.
- **Implementation:** write code and follow secure coding practices.
- **Testing:** verify functionality and look for defects, including security flaws.
- **Deployment:** release the software with appropriate configuration and access controls.
- **Maintenance:** patch issues, monitor behaviour, and respond to new threats.

Different organisations use different models, such as waterfall, agile, or DevOps-based processes. The labels differ, but teams still need to manage requirements, quality, security, and change.

## Security throughout the lifecycle

Threat modelling during design can identify likely abuse cases before code is written. Code review and automated testing can catch issues during implementation. Security testing before release can find weaknesses that earlier checks missed. After release, monitoring and patch management help respond to new problems.

No single tool catches everything. Automated scanners can produce false positives and miss logic flaws, so human review and testing remain important.

## What I took from this room

Security is not a final checkbox. It should influence requirements, design, implementation, testing, deployment, and maintenance. Finding a problem earlier usually makes it cheaper and easier to fix, which is one of the rare cases where planning ahead actually saves everyone time.