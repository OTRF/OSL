---
layout:     post
title:      "Getting Started with ATT&CK: Adversary Emulation and Red Teaming"
date:       2019-07-01 12:00:00
author:     "Blake Strom, Tim Schulz, y Katie Nickels"
categories: "ATT&CK"
tags:
  - ATT&CK
  - MITRE
  - CTI
  - INTEL
---

<p>We hope you have taken the time to read both Katie Nickels’s post on getting started using ATT&CK for threat intelligence and John Wunder’s post on using ATT&CK for detection and analytics! We’re here to bring you the third installment of the series, this time covering adversary emulation and red teaming with ATT&CK to demonstrate how we can test those new analytics John showed us how to build.</p>

<p>Continuing the theme of the previous entries, this post will be broken up by levels based on your team’s level of sophistication and what resources you have access to:</p>

<ul>
	<li><p>Level 1 for those just starting out who may not have many resources,</p></li>
	<li><p>Level 2 for those who are mid-level teams starting to mature, and</p></li>
	<li><p>Level 3 for those with more advanced cybersecurity teams and resources.</p></li>
</ul>

<p>For those unfamiliar with it, adversary emulation is a type of red team engagement that mimics a known threat to an organization by blending in threat intelligence to define what actions and behaviors the red team uses. This is what makes adversary emulation different from penetration testing and other forms of red teaming. Adversary emulators construct a scenario to test certain aspects of an adversary’s tactics, techniques, and procedures (TTPs). The red team then follows the scenario while operating on a target network in order to test how defenses might fare against the emulated adversary.</p>

<p>Since ATT&CK is a large knowledge base of real-world adversary behaviors, it doesn’t take much imagination to draw a connection between adversary or red team behaviors and ATT&CK. Let’s explore how security teams can utilize ATT&CK for adversary emulation to help improve their organization!
</p>

<h2 class="section-heading">Level 1</h2>

<p>Small teams and those mainly focused on defense can get a lot of benefit out of adversary emulation even if they don’t have access to a red team, so don’t worry! There are quite a few resources available to help jump-start testing your defenses with techniques that align with ATT&CK. We’ll highlight how you can dip your toe into adversary emulation by trying simple tests.</p>

<p>Atomic Red Team, an open source project maintained by Red Canary, is a collection of scripts that can be used to test how you might detect certain techniques and procedures mapped to ATT&CK techniques. For example, maybe you’ve followed Katie’s advice and looked at techniques used by APT3 such as Network Share Discovery (T1135). Your intel team passed this to your detection team, and following John’s guidance, they wrote a behavioral analytic to try to detect if an adversary performed this technique. But how do you know if you’d really detect that technique? Atomic Red Team can be used to test individual techniques and procedures to verify that behavioral analytics and monitoring capabilities are working as expected.</p>

<p>The Atomic Red Team repository has many atomic tests, each with a directory dedicated to the ATT&CK technique that is tested. You can view the full repository in the ATT&CK Matrix format.</p>

<p>To start testing, select the T1135 page to see the details and different types of atomic tests that are documented. Each of these tests contains information about what the technique is, the platforms supported, and how to execute the test.</p>

<img src="{{ site.baseurl }}/img/attomic_test_t1135.png" class="img-responsive" alt="T1135 Atomic Test Details">
<span class="caption text-muted">T1135 Atomic Test Details</span>

<p>We just executed our first Atomic Test! Once this is done, we can take a look to see if what we expected to detect was what we actually detected. For example, maybe we had a behavioral analytic in our Security Information and Event Management (SIEM) tool that should have alerted when “net view” executed, but we find it didn’t fire, so we figure out logs weren’t correctly being exported from our host. You troubleshoot and fix the problem, and now you’ve made a measurable improvement to help you have a better chance to catch an adversary using this procedure in the future. These singular tests allow for a laser focus on individual ATT&CK techniques, so it makes building ATT&CK-based defensive coverage easier to approach because you can start with a single test for a single technique and expand from there.</p>

<img src="{{ site.baseurl }}/img/attomic_testing_attack.png" class="img-responsive" alt="T1135 Atomic Test Details">
<span class="caption text-muted">Atomic Testing cycle with ATT&CK</span>

<p><em>Bonus Level 1.5 content: Got a process down for using Atomic Red Team to perform adversary emulation testing and ready for something that can help chain together sequences of behavior? Check out CALDERA next! CALDERA is an automated adversary emulation system created by MITRE that has many built-in behaviors mapped to ATT&CK techniques. It allows the operator to pick one technique or chain many together when building the test, which allows you to start to automate sequences of behaviors for your testing rather than manually executing single Atomic Tests. You can use one of the pre-built scenarios or define a more specific scenario by choosing the procedures (called abilities in CALDERA) that map to certain ATT&CK techniques you want to test.</em></p>

<h2 class="section-heading">Level 2</h2>

<p>For those of you out there who already have red team capabilities, you can get a lot out of integrating ATT&CK with your existing engagements. Mapping the techniques used in a red team engagement to ATT&CK provides a common framework when writing reports and discussing mitigations.</p>

<p>To get started, you could take an existing planned operation or tool you use and map it to ATT&CK. Mapping red team procedures to ATT&CK is similar to mapping threat intelligence to ATT&CK, so you might want to check out Katie’s recommendations for a 6-step process outlined in her threat intelligence post.</p>

<p>Luckily, sometimes mapping techniques can be as simple as searching the command used on the ATT&CK website. For example, if we’ve used the ‘whoami’ command in our red team operation, we can search that on the ATT&CK website and find that two techniques likely apply: System Owner/User Discovery (T1033) and Command-Line Interface (T1059).</p>

<img src="{{ site.baseurl }}/img/funcion_busqueda.png" class="img-responsive" alt="T1135 Atomic Test Details">
<span class="caption text-muted">Search function on <a href="https://attack.mitre.org">https://attack.mitre.org</a></span>

<p>Another helpful resource to get you started mapping red team procedures to ATT&CK is the APT3 Adversary Emulation Field Manual, which breaks out command-by-command actions that APT3 has used, all mapped to ATT&CK.</p>

<img src="{{ site.baseurl }}/img/adversary_manual.png" class="img-responsive" alt="T1135 Atomic Test Details">
<span class="caption text-muted">Excerpt from our “APT3 Adversary Emulation Field Manual”</a></span>

<p>If your red team is using tools like Cobalt Strike or Empire, good news — these are already mapped to ATT&CK. Armed with your individual commands, scripts, and tools mapped to ATT&CK, you can now plan your engagement.</p>

<p>Some red teams have their tried and true toolkits and methods of operation. They know what works because it works all the time. But what they don’t always know is how much of their tried and true TTPs overlap (or don’t!) with known threats that may target the organization. That leads to a bit of a gap in understanding how well the defenses stack up to what you’re actually trying to defend against, the adversaries targeting your environment and not necessarily the red team themselves.</p>

<p>We want to make sure we’re not just doing the techniques because our tool can perform them — we want to emulate a real adversary we care about to provide more value. For example, we could talk to our cyber threat intel (CTI) team and they tell us they’re concerned about targeting from the Iranian group known as OilRig. Since everything is structured in ATT&CK, we can use the ATT&CK Navigator to compare the techniques we could do with a tool we already have like Cobalt Strike to the techniques that we know OilRig has done based on open source reporting. (You can check out a demo of the Navigator here that shows how to do this.) In the below graphic, Cobalt Strike techniques are red, OilRig techniques are blue, and techniques Cobalt Strike can perform and OilRig has used are purple. These purple techniques give us a place to start to use a tool we already have and perform techniques that are a priority to our organization.</p>

<img src="{{ site.baseurl }}/img/cobalt_oilrig_overlap.png" class="img-responsive" alt="ATT&CK Matrix showing Cobalt Strike and OilRig technique overlap">
<span class="caption text-muted">ATT&CK Matrix showing Cobalt Strike and OilRig technique overlap</a></span>

<p>Aside from identifying overlap between Cobalt Strike and OilRig, the analysis can also show where there are opportunities to vary the red team’s behaviors beyond what they typically employ down to the procedure level. There may be cases where a technique is implemented in a particular way in the tools the red team uses, but an adversary isn’t known to perform it in that way. Having that knowledge helps the red team use different behaviors between tests to better cover what threats are known to do as part of the adversary emulation process.</p>

<p>At this point we could also to add in techniques we want to manually perform with commands or scripts. We could then add comments into the Navigator about the order we’ll execute the techniques in and how we will perform them.</p>

<p>While there are benefits to mapping to ATT&CK as we plan red team operations, we also reap the rewards once we’ve executed our operation as we communicate back to our blue team. If they are mapping analytics, detections, and controls back to ATT&CK, you can easily communicate with them in a common language about what you did and what they were successful at. Including an ATT&CK Navigator image (and even a saved Navigator layer) in a report can help this process and give them a template to improve upon.</p>

<em>Bonus Level 2.5 Content: After using ATT&CK to plan engagements and report results, try using the APT3 Emulation Plan or the ATT&CK Evaluations Round 1 scenario based on that plan to conduct an engagement emulating APT3 to show a baseline test against a particular adversary group.</em>

<h2 class="section-heading">Level 3</h2>

<p>By this point, your red team is integrating ATT&CK into operations and finding value in communicating back to the blue team. To advance your teams and the impact they’re having even more, you can collaborate with your organization’s CTI team to tailor engagements toward a specific adversary using data they collect by creating your own adversary emulation plan.</p>

<p>Creating your own adversary emulation plan draws on the greatest strength of combining red teaming with your own threat intelligence: the behaviors are seen from real-world adversaries targeting you! The red team can turn that intel into effective tests for showing what defenses work well and where resources are needed to improve. There is a much higher level of impact when visibility and controls gaps are exposed by security testing when you can show a high likelihood that they have been leveraged by a known adversary. Linking your own CTI to adversary emulation efforts will increase both the effectiveness of testing and the outputs to senior leadership to enact change.</p>

<p>We recommend a five-step process depicted in the below diagram to create an adversary emulation plan, execute the operation, and drive defensive improvements. (For a more detailed outline of the process, see the presentation by Katie Nickels and Cody Thomas on Threat-Based Adversary Emulation with ATT&CK.)</p>

<img src="{{ site.baseurl }}/img/creating_adv_plan.png" class="img-responsive" alt="Process for creating an adversary emulation plan">
<span class="caption text-muted">Process for creating an adversary emulation plan</a></span>

<ol>
	<li><p><strong>Gather threat intel</strong>— Select an adversary based on the threats to your organization and work with the CTI team to analyze intelligence about what the adversary has done. Combine what’s based on what your organization knows in addition to publicly available intel to document the adversary behaviors, what they go after, whether they do smash and grab or low and slow.</p></li>
	<li><p><strong>Extract techniques</strong>— In the same way you mapped your red team operations to ATT&CK techniques, map the intel you have to specific techniques in conjunction with your intel team. You could point your CTI team to Katie’s blog post to help them learn how to do this.</p></li>
	<li><p><strong>Analyze & organize</strong>— Now you have a bunch of intel about the adversary and how they operate, diagram that information into their operational flow in a way that’s easy to create specific plans from. For example, below is the operational flow the MITRE team created for the APT3 Adversary Emulation Plan.</p></li>

<img src="{{ site.baseurl }}/img/apt3_flow.png" class="img-responsive" alt="APT3 Operational Flow">
<span class="caption text-muted">APT3 Operational Flow</a></span>
	<li><p><strong>Develop tools and procedures</strong>— Now that you know what you’d like the red team to do, figure out how to implement the behavior. Consider:
		<ul>
			<li><p>How did the threat group use this technique?</p></li>
			<li><p>Did the group vary which technique used based on the environment context?</p></li>
			<li><p>What tools can we use to replicate these TTPs?</p></li>
		</ul>
	</p>
	</li>
	<li><p><strong>Emulate the adversary</strong>— With a plan in place, the red team now has the ability to execute and perform an emulation engagement. As we’ve recommended for all red team engagements using ATT&CK, the red team should closely work with the blue team to gain a deep understanding of where gaps are in the blue team’s visibility and why they exist.</p></li>

</ol>

<p>Once this entire process takes place, the red and blue teams can work with the CTI team to determine the next threat to repeat the process on, creating a continuous activity that tests defenses against real-world behaviors.</p>

<h2 class="section-heading">In Closing</h2>

<p>This blog post has showed you how to use ATT&CK for red teaming and adversary emulation, regardless of what resources you have (including if you don’t have a red team yet). We hope you’ve observed throughout this Getting Started series that each of these topics builds on the other, with threat intelligence informing the creation of analytics that can be validated and improved through adversary emulation — all while using the common language of ATT&CK. The final post will talk about performing assessments and engineering with ATT&CK, rounding out our Getting Started with ATT&CK series!</p>

<p>©2019 The MITRE Corporation. ALL RIGHTS RESERVED. Approved for public release. Distribution unlimited 19–00696–8.</p>

<p>Post original: <a href="https://medium.com/mitre-attack/getting-started-with-attack-red-29f074ccf7e3"><em>Getting Started with ATT&CK: Adversary Emulation and Red Teaming</em></a></p>