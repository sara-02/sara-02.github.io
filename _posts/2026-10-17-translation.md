---
layout: writing_post
title: Translation vs LLMs
categories: essay
---


This year, I had the opportunity to join the international book club run by a colleague named Sarah. As part of this book club, I read English translations of some very interesting books [1]. In all of these books, the author and translator notes shared one theme: "we are not trying to make a perfect translation; there are intentional gaps," And I asked why? Why make it hard on your English readers? Especially in the age of large language models (LLMs), when producing a well-polished English text is becoming cheaper. And the answer is often summarised as “in the gap; in the translation cracks is where culture lives.” This act of deliberation forces the reader to acknowledge another culture. Coincidentally, in my research life, I keep corroborating the same. With top-tier LLMs, a cursory assumption is that translation is a solved problem. Maybe it is, if you look at words or vocabulary accuracy, but what about context? What about translating meaning, beliefs, and implicit cultural references? [2]

A simple case in point: `saree: India, kimono: __` If your answer is `Japan,` then in terms of concept co-occurrence (saree, India) and (kimono, Japan) under the clothing entity, it is not wrong. But if you ask any sartorial expert, they will tell you that, unlike the saree, which is an everyday attire in the Indian subcontinent, kimonos are worn only on special occasions, rendering the analogy culturally and practically inapt. I only learned about this after exchanging notes with Japanese scholars while working on Project Camellia to benchmark LLMs on Asian cultural entities [3].

Beyond fill-in-the-blanks, these epistemic gaps become harder to find in long-form LLM responses. Ask an LLM to inform you about "dengue," and consider the nuggets of information (i.e. claims/factoids) it generates: who do they serve, and where does this information come from? We conducted a systematic study to establish this epistemic gap in LLMs. Spoiler alert: LLMs, while improving each year, still do not provide the same diversity of claims as a simple Google search [4] and gaps remain considerable for non-English claims.

Now, going back to our dengue example. A New York Times reporter looking for stats on dengue does not seek the same set of claims as someone in India looking for preventive measures for one of the prevalent diseases in our country. As Indians, we also know that dengue prevention solutions can include advice that doesn't fit all socioeconomic statuses. How do we account for this when training LLMs on dengue facts? Even with the same level of veracity or medical accuracy, dengue claims can still have varying degrees of utility for different demographics. Now, one might think that geo-tagging can personalise the set of claims for who the reader is… Then I would like to introduce you to the concept of intersectionality :P

We saw this in one of our studies. Asking an LLM to label something as hate speech received more hate labels if the persona assigned to the LLM was said to be a native speaker of the language. But when we explicitly prompted the LLM towards intersectionality, e.g. "assume you are a native Arabic speaker and a Muslim" vs "native Arabic speaker and a non-Muslim," we saw different error rates [5]. How LLMs can account for multiplicity of lived experiences is an open question. This lack of multi-objectivity in training LLMs to better handle subjective tasks came up in another study. We found that things like insider perspective adoption, extent of anthropomorphism, and adherence to Gricean maxims (which are themselves Western-centric) largely have orthogonal prevalence in LLM-generated responses to cultural queries even when looking only at English content [6].   

This epistemic and persona detour brings us back to translation and to one of our go-to sources of information, Wikipedia. During my onboarding week at my new job, I ran into a minor laptop issue, and my colleague Borge mentioned the term `Fejl 40` [7] and shared the Danish Wikipedia page on it. It refers to a common error when humans handle a device or equipment incorrectly, rather than a device fault. I read the 2-paragraph Wikipedia page after Google translating it, then checked the English version of the topic and saw it was much longer and didn't refer to the same example as the Danish page. The two Wikipedia pages currently serve two different audiences (the length of content on non-English wiki pages is a rant for another day). Still, for now, I am glad the Danish version doesn't exist as a mere translation of the longer English Wikipedia page, but as a source of local knowledge with references to Danish companies most associated with the error. Local references matter, lived experiences matter!

Unless data curators and LLM evaluators work with people close to this lived experience, we will count and measure data points that may not align with real-world everyday experiences. Meanwhile, for better or for worse, but mostly better, translation remains an unsolved problem for LLMs.

<hr style="border: 0; border-top: 1px dashed var(--global-text-color-light);">

<small><strong>Footnotes/References:</strong></small>

 1: So far I have read Heart Lamp (Kannada), Zazi in the Metro (French), The Three-Body Problem series (Mandarin), You Dreamed of Empires (Spanish), I Want to Kill Myself (Marathi).

 2: [Supporting and Performing Culture from the Inside](https://arxiv.org/abs/2609.32281)
 
 3: [Camellia: Benchmarking Cultural Biases in LLMs for Asian Languages](https://arxiv.org/abs/2510.05291)
 
 4: [What and Whose Knowledge? Measuring Epistemic Diversity in Large Language Models](https://arxiv.org/abs/2510.04226)
 
 5: [Hate Personified: Investigating the role of LLMs in content moderation](https://arxiv.org/abs/2410.02657)
 
 6: [Not What, But How: A Framework for Auditing LLM Responses across Positioning, Generalization, Anthropomorphism, and Maxims](https://arxiv.org/abs/2606.02493)
 
 7: [Danish - Fejl 40](https://da.wikipedia.org/wiki/Fejl_40), [English - User error](https://en.wikipedia.org/wiki/User_error)
