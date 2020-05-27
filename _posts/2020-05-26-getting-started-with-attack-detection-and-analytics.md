---
layout:     post
title:      "Getting Started with ATT&CK: Detection and Analytics"
date:       2019-07-01 12:00:00
author:     "John Wunder"
categories: "ATT&CK"
tags:
  - ATT&CK
  - MITRE
  - CTI
  - INTEL
---

<p>Hopefully you had a chance to read Katie Nickels’s post on getting started using ATT&CK for threat intelligence, which walked through understanding what adversaries are doing to attack you and how to use that knowledge to prioritize what to defend. In this post, I’ll talk about how to build detections for those behaviors.</p>

<p>As with the first post of the series, this post will be broken up by levels based on how sophisticated your team is and what resources you have access to:</p>

<ul>
	<li><p>Level 1 for those just starting out who may not have many resources,</p></li>
	<li><p>Level 2 for those who are mid-level teams starting to mature, and</p></li>
	<li><p>Level 3 for those with more advanced cybersecurity teams and resources.</p></li>
</ul>

<p>Building analytics to detect ATT&CK techniques might be different than how you’re used to doing detection. Rather than identifying things that are known to be bad and blocking them, ATT&CK-based analytics involve collecting log and event data about the things happening on your systems and using that to identify the suspicious behaviors that are described in ATT&CK.</p>

<h2 class="section-heading">Level 1</h2>

<p>The first step to creating and using ATT&CK analytics is understanding what data and search capabilities you have. In order to find suspicious behaviors, after all, you need to be able to see what’s happening on your systems. One way to do this is to look at the Data Sources listed for each ATT&CK technique. Those data sources describe the types of data that could give you visibility into the given technique. In other words, they give you a good starting point for what to collect.</p>

<img src="{{ site.baseurl }}/img/data_sources.png" class="img-responsive" alt="Data sources for an ATT&CK technique">
<span class="caption text-muted">Data sources for an ATT&CK technique</span>

<p>If you look through the data sources for a bunch of different techniques, or follow the approach Roberto Rodriguez and Jose Luis Rodriguez demonstrated at ATT&CKCon to look across techniques at data sources (MITRE also created some helper scripts), you’ll notice that several sources are valuable at detecting a large number of techniques:</p>

<ul>
	<li><p>Process and process command line monitoring, often collected by Sysmon, Windows Event Logs, and many EDR platforms</p></li>
	<li><p>File and registry monitoring, also often collected by Sysmon, Windows Event Logs, and many EDR platforms</p></li>
	<li><p>Authentication logs, such as those collected from the domain controller via Windows Event Logs</p></li>
	<li><p>Packet capture, especially east/west capture such as that collected between hosts and enclaves in your network by sensors such as Zeek</p></li>
</ul>

<p>Once you know what data you have, you’ll need to collect that data into some kind of search platform (SIEM) so you can run analytics against it. You might already have this as part of your IT or security operations, or it might be something new you need to build. For these screenshots and the walkthrough, I’ll be using ELK (ElasticSearch/Logstash/Kibana) with Sysmon data, but there are a number of commercial and open source offerings and we don’t recommend any specific platform. Don’t underestimate these steps in the process, tuning your data collection is often the hardest part!</p>

<p><em>Bonus Level 0 Content: Need access to a good enterprise dataset for testing? Check out the Boss of the SOC (BOTS) dataset from Splunk or the BRAWL dataset from MITRE. Both are available as JSON and so can be loaded into Splunk, ELK, and other SIEMs. BOTS is very extensive and contains real noise, while BRAWL is much more constrained and focuses only on the red team activity.</em></p>

<p>Once you’ve got data in your SIEM you’re ready to try some analytics. One great starting point is to look at analytics created by others and run them against your data. There are several analytic repositories listed in the resources below, but a good starter analytic if you have endpoint process data is CAR-2016–03–002. That will try to find usage of WMI to execute commands on remote systems, a common adversary technique described by Windows Management Instrumentation.</p>

<h2 class="section-heading">Level 1</h2>

<img src="{{ site.baseurl }}/img/car_remote_process.png" class="img-responsive" alt="CAR entry for Create Remote Process via WMIC">
<span class="caption text-muted">CAR entry for Create Remote Process via WMIC</span>

<p>You’ll want to read and understand the description to know what it’s looking for, but the important part to get it running is the pseudocode at the bottom. Translate that pseudocode into a search for whatever SIEM you’re using (making sure the field names in your data are correct) and you can run it to get results. If you’re not comfortable translating the pseudocode, you can also use an open source tool called Sigma and its repository of rules to translate to your target. In this case, CAR-2016–03–002 is included in a Sigma rule already, if you’ve installed Sigma and you’re in its directory you can run this command to get (as an example) the ELK/WinLogBeats query:</p>

<code>sigmac --target es-qs -c tools/config/winlogbeat.yml rules/windows/process_creation/win_susp_wmi_execution.yml</code>

<img src="{{ site.baseurl }}/img/WMIanalytic.png" class="img-responsive" alt="Results from running WMI analytic against BRAWL data">
<span class="caption text-muted">Results from running WMI analytic against BRAWL data</span>


<p>Your job now is to look through each result and figure out whether it’s malicious. If you used the BRAWL dataset, it’s all pretty malicious: it tries to run and.exe, and upon further exploring the related events, and.exe had just been moved to that host over SMB and added to the autorun registry keys for persistence. If you’re looking at your own enterprise data, it’s hopefully benign or known red team data — if not, maybe stop reading this blog and figure out what you’re dealing with.</p>

<p>Once you have the basic search returning data and feel comfortable that you can understand the results, try to filter out the false positives in your environment so that you don’t overwhelm yourselves. Your goal shouldn’t be to get to zero false positives, but it should be to reduce them as much as possible while still ensuring that you’ll catch the malicious behavior. Once the analytic has a low false-positive rate, you can automate creating a ticket in your SOC each time the analytic fires or adding it to a library of analytics to use for manual threat hunting.</p>

<h2 class="section-heading">Level 2</h2>

<p>Once you have analytics other people wrote in operations, you can start expanding coverage by writing your own analytics. This is a more complicated process that requires understanding how the attacks work and how they get reflected in the data. To start, look at the technique description from ATT&CK and the threat intel reports linked in the examples.</p>

<p>As an example, let’s pretend there were no good detections for Regsvr32. The ATT&CK page lists several different variants for how Regsvr32 is used. Rather than writing one analytic to cover all of them, focus in on just one aspect in order to avoid spinning your wheels. For example, you might want to detect the “Squiblydoo” variant that was discovered by Casey Smith at Red Canary. The reports linked from the examples show several instances of command lines where regsvr32 was used, such as this example from the Cybereason analysis of Cobalt Kitty:</p>

<img src="{{ site.baseurl }}/img/squiblydoo.png" class="img-responsive" alt="Results from running WMI analytic against BRAWL data">
<span class="caption text-muted">Evidence of Squiblydoo used by Cobalt Kitty</span>

<p>Once you understand how adversaries use the technique, you should figure out how to run it yourself so you can see it in your own logs. An easy way to do that is to use Atomic Red Team, an open source project led by Red Canary that provides red team content aligned to ATT&CK that can be used to test analytics. For example, you can find their list of attacks for Regsvr32, including Squiblydoo. Of course, if you’re already doing red-teaming, feel free to run the attacks you know yourself (on systems where you have permission!) and try to develop analytics for those!</p>

<p><em>Bonus Level 0 Content: Really want to create your own analytics and run your own attacks but don’t have your own network? Stand up a VM and monitor it as above, then run the attacks on that. Detection Lab provides a good set of configuration scripts to do just that.</em></p>

<img src="{{ site.baseurl }}/img/squiblydoo_output.png" class="img-responsive" alt="Results from running WMI analytic against BRAWL data">
<span class="caption text-muted">Output from running the Squiblydoo attack to launch calc.exe</span>

<p>Once you’ve run the attack, look inside your SIEM to see what log data was generated. At this stage, you’re looking for things that make this malicious event look distinctive. I picked Squiblydoo as an example because it’s an easy one: there’s no legitimate reason to have regsvr32.exe call out to the Internet, so a simple analytic is to look for times when the regsvr32.exe process is created and the command line includes “/i:http”.</p>

<p>A general pattern to follow is to write the search to detect malicious behavior, revise it to filter out false positives, make sure it still detects the malicious behavior, and then repeat to reduce other sorts of false positives.</p>

<img src="{{ site.baseurl }}/img/analytic_workflow.png" class="img-responsive" alt="Analytic development workflow">
<span class="caption text-muted">Analytic development workflow</span>


<h2 class="section-heading">Level 3</h2>

<p>Feel confident that you’re cranking out quality analytics to detect attacks from Atomic Red Team? Test that confidence and improve your defenses by doing some purple teaming!</p>

<p>In the real world, adversaries don’t just carry out cookie cutter attacks copy/pasted from some book. They adapt and try to evade your defenses — including your analytics (that’s why there’s a defense evasion tactic in ATT&CK, after all). The best way to ensure that your analytics are robust against evasion is to work directly with a red teamer. You and your blue team will be responsible for creating analytics and the red team will be responsible for <em>adversary emulation</em> — essentially, trying to evade your analytics by executing the types of attacks and evasions that we know from threat intelligence that adversaries use in the real world. In other words, they’ll act like real adversaries so that you can understand how your analytics will fare against real adversaries.</p>

<p>Here’s how that might work in practice. You have some analytic, let’s say to detect credential dumping. Maybe you heard about mimikatz and write an analytic to detect mimikatz.exe on the command line or Invoke-Mimikatz via Powershell. In order to purple team this, give that analytic to your red team. They can then find and execute an attack that will evade that analytic. In this case, they might rename the executable to mimidogz.exe. At that point, you’ll need to update your analytic to look for different artifacts and behaviors that won’t rely on the exact naming. Perhaps you look for the specific GrantedAccess bitmask from when mimikatz accesses lsass.exe (don’t worry about the exact details, this is just an example). You’ll again give this to your red team, and they’ll execute an evasion that, for example, adds an additional access so that your GrantedAccess bitmask no longer detects it. This back and forth is known as purple teaming, and it’s a great way to rapidly improve the quality of your analytics because it measures your ability to detect the attacks that adversaries actually use. Once you get to a stage where you’re purple teaming all of your analytics, you can even automate the process to make sure you don’t have any regressions and are catching new variants of attacks. We’re working on a post just like this one talking more about adversary emulation and red-teaming —so stay tuned to learn much more about that half of the process.</p>

<p>This is also related to what Andy Applebaum will talk about in a future blog post on ATT&CK SOC Assessments. Once you’re this advanced and are building out a corpus of analytics, you’ll want to use ATT&CK (either via the ATT&CK Navigator or using your own tools) to track what you can and can’t cover. Maybe, for example, you start with a wish-list of analytics to detect the techniques that Katie Nickels and Brian Beyer point out in their SANS CTI Summit presentation.</p>

<img src="{{ site.baseurl }}/img/heatmap.png" class="img-responsive" alt="Analytic development workflow">
<span class="caption text-muted">Heatmap with targeted techniques</span>

<p>Then, you integrate the analytics from CAR and color those yellow to indicate that at least you have some coverage (as indicated above, a single analytic is unlikely to provide sufficient coverage for any given technique).</p>

<img src="{{ site.baseurl }}/img/heatmap_with_car.png" class="img-responsive" alt="Analytic development workflow">
<span class="caption text-muted">Heatmap with CAR analytics</span>

<p>Then, you refine those analytics and maybe add more to improve your coverage for those techniques. Eventually, maybe you’re comfortable enough with your detection for some of them that you color them green — just keep in mind that you’ll never be 100% sure of catching every usage of a given technique, so green doesn’t mean done, it just means OK for now.</p>

<img src="{{ site.baseurl }}/img/heatmap_with_car_custom.png" class="img-responsive" alt="Analytic development workflow">
<span class="caption text-muted">Heatmap with CAR and custom-developed analytics</span>

<p>And of course, over time, you’ll want to expand the scope of the things that you care about. You can reference back to Katie’s post on prioritizing by threat actor, use some of the resources published by vendors to prioritize based on prevalence of the technique based on their monitoring, or perhaps best of all, develop analytics for the activity that you know about from your own incidents. In the end, you want to be developing a more and more comprehensive set of detections so that you can detect more and more of the things that adversaries do to attack us — and ATT&CK gives you the scorecard to do so.</p>

<h2 class="section-heading">In Closing</h2>

<p>This blog post gave you an idea of what it means to build analytics to detect ATT&CK techniques, as well as how to think about building out a suite of analytics. It builds on the previous post to show not just that you can understand what the adversary can do via cyber threat intelligence, but that you can use that intelligence to build analytics to detect those techniques. Future posts will talk more about how to build an engineering and assessments process for your defenses, including analytics, and how to do comprehensive red-teaming to validate your defenses.</p>

<p><a href="https://medium.com/mitre-attack/getting-started-with-attack-detection-a8e49e4960d0"></a><em>Getting Started with ATT&CK: Detection and Analytics</em></p>