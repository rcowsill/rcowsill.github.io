# Security - docker-library/official-images
## CI/CD vulnerability report and fixes

The [docker-library/official-images](https://github.com/docker-library/official-images) repository is the 'Primary source
of truth for the Docker "Official Images" program'. This program is a curated set of Docker repositories hosted on
[Docker Hub](https://hub.docker.com/u/library).

In late 2021 I found a vulnerability in one of the GitHub actions workflows used in the official-images repo. Exploiting it would
have allowed an attacker to gain `content: write` permission, with which they could directly commit changes to the repo.
That access could then have been used to stage a supply chain attack.

For full details, see my [original report](original-report.md) sent to the official-images maintainers (reproduced here with their kind permission).

My sincere thanks to the official-images maintainers for their rapid response and willingness to collaborate on fixing the issue.

### Disclosure Timeline:
* **2021-11-23:** Sent [report](original-report.md) to security@docker.com
* **2021-11-24:** official-images maintainers replied and created a temporary private fork for collaboration
* **2021-11-25:** Initial fix PR made in the private fork ([docker-library/official-images@16ef34a](https://github.com/docker-library/official-images/commit/16ef34a23aab4aef394a9f92b92fce99e4c2f348))
* **2021-11-25:** Possible bypass of initial fix found in testing
* **2021-11-26:** Supplemental fix provided ([docker-library/official-images@d0c3564](https://github.com/docker-library/official-images/commit/d0c35646ec198e388748b8967d702f94a3abed46))
* **2021-11-29:** Both fixes reviewed by official-images maintainers
* **2021-11-30:** Both fixes merged into docker-library/official-images ([docker-library/official-images@7651ab3](https://github.com/docker-library/official-images/commit/7651ab3bc5d4e98c6de5b95173b6a74c07e6f09a))
