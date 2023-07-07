# Common Bugs in Writing

Preface (suggested by Bob Briscoe)

- Be clear what you're trying to say before you write it.
- Don't get attached to words you have written; be prepared to scrap what you wrote while you were thrashing around trying to work out what you wanted to say, even if its a whole paper.

Like almost all rules, there are cases where breaking them is a good idea and seasoned writers may well object with "but" responses for some of these.  Thus, consider the rules below as mental rumble strips - you should probably slow down and think if you encounter these cases.

1. Avoid use of passive tense if at all possible.  Example:  "In each reservation request message, a refresh interval used by the sender is included." reads better and shorter as "Each ...  message includes ..."

2. Use strong verbs instead of lots of nouns and simple terms rather than fancy-sounding ones. Examples:

   | verbose, weak verbs, bad | short, strong, good |
   | ------------------------ | ------------------- |
   | make assumption          | assume              |
   | is a function of         | depends on          |
   | is an illustration       | illustrates, shows  |
   | is a requirement         | requires, need to   |
   | utilizes                 | uses                |
   | had difference           | differed            |

3. If you find yourself saying "In other words," it means you didn't say it clearly enough the first time.  Go back and rewrite the first attempt.

4. Avoid filler words, e.g., by converting sentences into a simple actor-action-object phrasing.

5. Check for missing articles, particularly if your native tongue doesn't have them.  Roughly, concepts and classes of things don't, most everything else more specific does.  ("Routers route packets.  The router architecture we consider uses small rodents.") Don't use articles in front of proper nouns and names ("Internet Explorer is a popular web browser.  The current version number is 5.0.  Bill Gates did not write Internet Explorer.") [NEED POINTER HERE]

6. Each sentence in a paragraph must have some logical connection to the previous one.  For example, it may describe an exception ("but," "however"), describe a causality ("thus," "therefore," "because of this"), indicate two facets of an argument ("on the one hand," "on the other hand"), enumerate sub-cases ("first," "secondly") or indicate a temporal relationship ("then," "afterwards").  If there are no such hints, check if your sentences are indeed part of the same thought.  A new thought should get its own paragraph, but still clearly needs some logical connection to the paragraphs that preceded it.

7. Protocol abbreviations typically do not take an article, even if the expanded version does.  For example, "The Transmission Control Protocol delivers a byte stream" but "TCP delivers a byte stream," since it an abstract term. ("The TCP design has been successful." is correct since the article refers to the design, not TCP.)

8. Note that abbreviations for organizations do take a definite article, as in "The IETF standardized TCP."

9. Since the "P" in TCP, UDP and similar abbreviations already stands for "protocol," saying the "the TCP protocol" is redundant, albeit common.  (LCD, Liquid Crystal Display, is another common case where many are tempted to incorrectly write LCD display.  Indeed, Google references 2,060,000 instances of that usage.)

10. Use consistent tense - present, usually, unless reporting results achieved in earlier papers.

11. **None**:  None can take either singular or plural verbs, depending on the intended meaning (or taste).  Both *none of these mistakes are common* and *none of these mistakes is common* are [correct](http://www.quinion.com/words/qa/qa-non2.htm), although other sources [only lists the singular](http://www.llrx.com/columns/grammar1.htm) and [The Tongue untied](http://grammar.uoregon.edu/agreement/singplr.html) makes finer distinctions based on whether it refers to a unit or a measure.

12. Use hyphens for concatenated words:  "end-to-end architecture," "real-time operating system" (but "the computer may analyze the results in real time"), "per-flow queueing," "flow-enabled," "back-to-back," ...

13. In general, hyphens are used

14. - adding prefixes that would result in double vowels (except for co-, de-, pre-, pro-), e.g., supra-auditory;
    - all-: all-around, all-embracing;
    - half-: half-asleep, half-dollar (but halfhearted, halfway);
    - quasi-: quasi-public
    - self-: self-conscious, self-seeking (but selfhood, selfless)
    - to distinguish from a solid homograph, e.g., re-act vs. react, re-pose vs. repose, re-sign vs. resign, re-solve vs. resolve, re-lease vs. release
    - A compound adjective made up of an adjective and a noun in combination should usually be hyphenated.  (WiT, p.  230) Examples:  cold-storage vault, hot-air heating, short-term loan, real-time operating system, application-specific integrated circuit, Internet-based.
    - words ending in -like when the preceding word ends in 'l', e.g., shell-like

15. Don't overuse dashes for separation, as they interrupt the flow of words.  Dashes may be appropriate where you want to contrast thoughts very strongly or the dash part is a surprise of some sort.  Think of it as a very long pause when speaking.  In many cases, a comma-separated phrase works better.  If you do use a dash, make sure it's not a hyphen (- in LaTeX), but an em-dash (--- in LaTeX).

16. Avoid [*scare quotes*](http://www.cogs.susx.ac.uk/local/doc/punctuation/node31.html), as they indicate that the writer is distancing himself from the term or the term is meant to be ironic.

17. Numbers ten or less are spelled out: "It consists of three fields," not "3 fields".

18. Use until instead of the colloquial *till*.

19. Use. *Eq. 7*, not *Equation (7)*, unless you need to fill empty pages.

20. *Optimal* can't be improved - *more optimally* should be *better* or maybe *more nearly optimal*.

21. Avoid in-line enumeration like:  "Packets can be (a) lost, (b) stolen, (c) get wet." The enumeration only interrupts the flow of thought.  (There are exceptions, e.g., when you later refer to these cases.)

22. Avoid itemization (bullets) in most cases, as they take up extra space and make the paper read like PowerPoint slides.  Bullets can, however, be used effectively to emphasize key points, if used sparingly.  If you want to describe components or algorithms, often the LaTeX description environment works better, as it highlights the term, providing a low-level section delineation.

23. Avoid bulleted lists of one-sentence paragraphs.  They make your paper look like a slide presentation and interfere with smooth reading.

24. Instead of "Reference [1] shows" or "[1] shows," use "Smith [1] showed" or "Smith and Jones [1] showed" or "Smith *et al.* [1] showed" (if more than two authors).  ["et al."](http://www.sparknotes.com/writing/style/topic_76.html) is generally used for papers with more than two authors.  (Note that "et al." makes the subject plural if the authors are the subject of the sentence [rather than the paper](http://english.stackexchange.com/questions/99886/is-et-al-used-as-a-singular-or-plural-subject), so it is "Smith et al.  [1] show" not "shows".  Some authors prefer to treat the article as the, singular, subject, but this may sound peculiar to some readers, so the construction is probably best avoided or rephrased to make the article more clearly the subject. For example, "Section 3 of Smith and Wesson [42] claims that..." or "Smith and Wesson claim that ... [42]" or "In [42], Smith and Wesson claim that...".) Or, alternatively, "the foobar protocol [1] is an example ...".  This keeps the reader from having to flip back to the references, as they'll recognize many citations by either author name or project name.  No need to refer to RFC numbers in the text (except in RFCs and Internet Drafts).  Exception for very low-level presentation:  "RFC822-style addresses". While there is no firm rule, it seems preferable to include the reference immediately after the author name, rather than at the end of the material describing the citation. This keeps the reader from having to look ahead on encountering the reference and avoids inconsistent phrasing and guessing when multiple papers are cited for a single fact, as in "Hertz [1] and Marconi [2] worked on radio waves", rather than "Hertz and Marconi worked on radio waves [1,2]."

25. Use normal capitalization in captions ("This is a caption," not "This is a Caption") unless your style guide requires heading-style capitalization.

26. All headings must be capitalized consistently, either in heading style, capitalizing words, or sentence style, across all levels of headings.

27. Parentheses or brackets are always surrounded by a space:  "The experiment(Fig.  7)shows" is wrong; "The experiment (Fig.  7) shows" is right.

28. Avoid excessive parenthesized remarks as they make the text hard to read; fold into the main sentence.  Check whether the publication allows footnotes - some magazines frown upon them.  More than two footnotes per page or a handful per paper is a bad sign.  You probably should have applied to law school instead.

29. The material should make just as much sense without the footnotes. If the reader constantly has to look at footnotes, they are likely to lose their original place in the text. As a matter of taste, I find URLs better placed in the references rather than as a footnote, as the reader will know that the footnote is just a reference, not material important for understanding the text.

30. There is no space between the text and the superscript for the footnote.  I.e., in LaTeX, it's `text\footnote{}` rather than `text \footnote{}`.

31. Check that abbreviations are always explained before use.  Exceptions, when addressed to the appropriate networking audience:  ATM, BGP, ftp, HTTP, IP, IPv6, RSVP, TCP, UDP, RTP, RIP, OSPF, BGP, SS7.  Be particularly aware of the net-head, bell-head perspective.  Even basic terms like PSTN and POTS aren't taught to CS students...  For other audiences, even terms like ATM are worth expanding, as your reader might wonder why ATM has anything to do with cells rather than little green pieces of paper.

32. Never start a sentence with "and". (There are exceptions to this rule, but these are best left to English majors.)

33. Don't use colons (:) in mid-sentence. For example, "This is possible because: somebody said so" is wrong - the part before the colon must be a complete sentence.

34. Don't start sentences with "That's because".

35. In formal writing, contractions like *don't*, *doesn't*, *won't* or it's are generally avoided.

36. Be careful not to confuse `its` with `it's` (it is).

37. Vary expressions of comparison:  "Flying is faster than driving" is much better than "Flying has the advantage of being faster" or "The advantage of flying is that it is faster.".

38. Don't use slash-constructs such as "time/money".  This is acceptable for slides, but in formal prose, such expressions should be expanded into "time or money" or "time and money," depending on the meaning intended.

39. Avoid [cliches](http://www.squidoo.com/businesscliches) like "recent advances in ...," "exponential growth," and "paradigm".  You do not want readers of your work to play [buzzword bingo](http://lurkertech.com/buzzword-bingo/).  Other words should be [banished](http://www.lssu.edu/banished/).

40. Don't use symbols like "+" (for "and"), "%" (for "fraction" or "percentage") or "->" (for "follows" or "implies") in prose, outside of equations.  These are only acceptable in slides.

41. Avoid capitalization of terms.  Your paper is not the U.S.  Constitution or Declaration of Independence. Technical terms are in lower-case, although some people use upper case when explaining an acronym, as in "Asynchronous Transfer Mode (ATM)".

42. Expand all acronyms on first use, except acronyms that every reader is expected to know. (In a research paper on TCP, expanding TCP is probably not needed - somebody who doesn't know what TCP stands for isn't likely to appreciate the rest of the paper, either.)

43. Each paragraph should have a lead sentence summarizing its content. If this doesn't work naturally, the paragraph is probably too short. Try reading just the first lines of each paragraph - the paper should still make sense. For example,

    > There are two service models, integrated and differentiated service.  Integrated service follows the German approach that anything that isn't explicitly allowed is verboten.  It strictly regulates traffic, but also makes the trains run on time.  Differentiated service follows the Animal Farm appraoch, where some traffic is more equal than others.  It seems simpler, until one has to worry about proletariat traffic dressing up as the aristocracy.

44. `$i$th`, not `$i-th$`.

45. Units are always in roman font, never *italics* or LaTeX math mode.  Units are set off by one (thin) space from the number.  In LaTeX, use ~ to avoid splitting number and units across two lines.  `\;` or `\,` produces a thin space.

46. For readability, powers of a 1,000 are divided by commas.

47. Use "kb/s" or "Mb/s," not "kbps" or "Mbps" - the latter are not scientific units.  Be careful to distinguish "Mb" (Megabit) and "MB" (Megabytes), in particular "kb" (1,000 bits) and "KB" (1,024 bytes).

48. It's always kHz (lower-case k), not KHz or KHZ.

49. It's Wi-Fi, not WiFi (or wifi), since this is a trademark.

50. Operating systems such as Android, (Mac)OSX, Linux or Windows are typically capitalized.

51. It is not common to use the trademark symbol ® (or its country cousins SM and TM) unless you are the owner of the mark and then only on [first use](http://www.forbes.com/sites/work-in-progress/2014/03/12/when-and-how-do-i-have-to-use-trademark-symbols/).

52. Units and Measurements

53. , 

54. Taligent style guide

55. Use "ms," not "msec," for milliseconds.

56. Use "0.5" instead of ".5," i.e., do not omit the zero in front of the decimal point. (*Words into Type* recommends that "for quantities less than one, a zero should be set before the decimal point except for quantities that never exceed one.")

57. Avoid "etc."; use "for example," "such as," "among others" or, better yet, try to give a complete list (unless citing, for example, a list of products known to be incomplete), even if abstract. See also Strunk and White:

    > Etc.:  Not to be used of persons.  Equivalent to **and the rest, and so forth**, and hence not to be used if one of these would be insufficient, that is, if the reader would be left in doubt as to any important particulars.  Least open to objection when it represents the last terms of a list already given in full, or immaterial words at the end of a quotation.  At the end of a list introduced by **such as**, **for example**, or any similar expression, etc.  is incorrect. 

58. If you say, "for example" or "like," do not follow this with "etc.". Thus, it's "fruit like apples, bananas and oranges". The "like" and "for example" already indicate that there are more such items.

59. Avoid excessive use of "i.e.".  Vary your expression:  "such as," "this means that," "because," ....  "I.e." is not the universal conjunction!

60. Remember that "i.e." and "e.g." are *always* followed by a comma.

61. Do not use ampersands (&) or slash-abbreviations (such as s/w or h/w) in formal writing; they are acceptable for slides.

62. "respectively" is preceded by a comma, as in "The light bulbs lasted 10 and 100 days, respectively."

63. *Therefore*, *however*, *hence* and *thus* are usually followed by a comma, as in "Therefore, our idea should not be implemented."

64. Never use "related work**s**" unless you are talking about works of art.  It's "related work".

65. Similarly, "code**s**" refer to encryption keys, not multiple programs. You would say "I modified multiple programs," not "multiple codes".

66. Use "in Figure 1" instead of "following figure" since figures may get moved during the publication or typesetting process.  Don't assume that the LaTeX figure stays where you put it.

67. Text columns in tables are left-aligned, numeric columns are aligned on the decimal or right-aligned.

68. Section, Figure and Table are capitalized, as in "As discussed in Section 3".  Figure can be abbreviated as Fig., but the others are not usually abbreviated, but that's a matter of taste - just be consistent.

69. Section titles are *not* followed by a period.

70. In LaTeX, tie the figure number to the reference, so that it doesn't get broken across two lines:

    ```
    Fig.~\ref{fig:arch}
    ```

71. Do not use GIF images for figures, as GIFs produce horrible print quality and are huge.  Export into PostScript.  At that stage, you'll learn to "appreciate" Microsoft products.  `xfig` and `gnuplot` generally produce PostScript that can be included without difficulties.

72. Only use line graphs when you are trying to show a functional or causal relationship between variables.  When showing different experiments, for example, use bar graphs or scatter plots.

73. Figures show, depict, indicate, illustrate. Avoid "(refer to Fig. 17)". Often, it is enough to simply put the figure reference in parenthesis: "Packet droppers (Fig. 17) have a pipe to the bit bucket, which is emptied every night."

74. If you quote something literally, enclose it in quotation marks or show it indented and in smaller type ("block quote").  A mere citation is not sufficient as it does not tell the reader whether you simply derived your material from the cited source or copied it verbatim.

75. Do not refer to colors in graphs.  Many people will print the paper on a monochrome (black and white) printer and will have no idea what you are talking about when referring to the red or blue line.  Also, 1 in 12 men and 1 in 200 women are color blind.  Thus, make sure that graph lines are easily distinguishable when printing on a monochrome printer.  "Use both color and shape to convey the same meaning; for example, solid and dashed lines or different fill patterns can help readers understand the figure without relying solely on color.  Each line of your line graph should be a thick line with a unique data point symbol.  Connect the data label to the data line rather than relying on a color key." (IEEE)

76. Avoid numbers with artificial precision.  Unless you have done enough experiments to be sure that the value measured is indeed meaningful to five digits after the decimal point, you're overstating your results.

77. Do not forget to acknowledge your funding support.  If you do forget, you may not have any to acknowledge in the future.

78. All references must use consistent capitalization for the paper titles, i.e., either all title-case or all sentence-case.

79. Technical report citations must have the name of the organization such as the university or company.  Conferences must cite the location.

80. Check your references to make sure they are up to date. For example, Internet Drafts might have been replaced by RFCs and technical reports or workshop papers by conference or journal papers.

81. References should be consistent: all authors should either be given with their full name (John Doe) or abbreviated (J. Doe), but not combinations.

82. Conference references should contain the location of the conference, the month and some indication such as "Proc.  of" or "Conference".  Journal references always contain the volume, issue number and pages. It must be obvious from the citation whether an article was in a journal or in a conference.

83. Only include the year of the publication once, rather than multiple times in different contexts.  For example, avoid something like "A.  Doe, "Performance of IEEE 802.11ac in a Rayleigh channel," in *Proc.  2015 ACM Intern.  Conf.  on Mobile Computing*, Rome, Italy, May 2015.  ACM Press, 2015.

See also [University of Minnesota Style Manual](http://www1.umn.edu/urelate/style/language-usage.html).  Many of these issues are also described in more detail in the US Federal [plain language guidelines](http://www.plainlanguage.gov/), [humorously](http://www.plainlanguage.gov/examples/humor/writegood.cfm), and, with a UK origin, [*The Complete Plain Words*](http://en.wikipedia.org/wiki/The_Complete_Plain_Words), dating back to 1954.  Similar points are made also by [John Owens](http://www.ece.ucdavis.edu/~jowens/commonerrors.html).

Thanks to Christian Bettstetter for contributions.

## References

- https://www.cs.columbia.edu/~hgs/etc/writing-bugs.html