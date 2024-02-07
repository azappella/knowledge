# LLM

# Purpose

Bootstrap knowledge of LLMs ASAP. With a bias/focus to GPT.

Avoid being a link dump. Try to provide only valuable well tuned information.

## Prelude

Neural network links before starting with transformers.

* https://www.youtube.com/watch?v=aircAruvnKk&list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi
* https://www.3blue1brown.com/topics/neural-networks
* http://neuralnetworksanddeeplearning.com/

## Key

* 🟢 = easy, 🟠 = medium, 🔴 = hard
* 🕰️ = long, 🙉 = low quality audio

## Youtube Lessons

* 🟢🕰️ **Łukasz Kaiser** [Attention is all you need; Attentional Neural Network Models](https://www.youtube.com/watch?v=rBCqOTEfxvg) This talk is from 6 years ago.
* 🟢🕰️ **Andrej Karpathy** [The spelled-out intro to language modeling: building makemore](https://www.youtube.com/watch?v=PaCmpygFfXo): basic. bi-gram name generator model by counting, then by NN. using pytorch.
* 🟢🕰️ **Andrej Karpathy**  [Building makemore Part 2: MLP](https://www.youtube.com/watch?v=TCH_1BHY58I): 
* 🕰️ **Andrej Karpathy**  [Building makemore Part 3: Activations & Gradients, BatchNorm](https://www.youtube.com/watch?v=P6sfmUTpUmc)): 
* 🕰️ **Andrej Karpathy**  [Building makemore Part 4: Becoming a Backprop Ninja](https://www.youtube.com/watch?v=q8SA3rM6ckI): 
* 🟢 **Andrej Karpathy** [State of GPT](https://build.microsoft.com/en-US/sessions/db3f4859-cd30-4445-a0cd-553c3304f8e2)
* 🟢 **Hedu AI** [Visual Guide to Transformer Neural Networks - (Episode 1) Position Embeddings](https://www.youtube.com/watch?v=dichIcUZfOw): Tokens are embedded into a semantic space. sine/cosine position encoding explained very well.
* 🟢 **Hedu AI** [Visual Guide to Transformer Neural Networks - (Episode 2) Multi-Head & Self-Attention](https://www.youtube.com/watch?v=mMa2PmYJlCo): Clear overview of multi-head attention.
* 🟢 **Hedu AI** [Visual Guide to Transformer Neural Networks - (Episode 3) Decoder’s Masked Attention](https://www.youtube.com/watch?v=gJ9kaJsE78k): Further details on the transformer architecture.
* 🟠🕰️ **Andrej Karpathy**  [Andrej Karpathy - Let's build GPT: from scratch, in code, spelled out.](https://www.youtube.com/watch?v=kCc8FmEb1nY): build up a Shakespeare gpt-2-like from scratch. starts with bi-gram and adds features one by one. pytorch.
* 🔴🕰️ **Chris Olah** [CS25 I Stanford Seminar - Transformer Circuits, Induction Heads, In-Context Learning](https://www.youtube.com/watch?v=pC4zRb_5noQ): Interpretation. Deep look into the mechanics of induction heads. [Companion article](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html)
* 🟢 **Jay Alammar** [The Illustrated Word2vec - A Gentle Intro to Word Embeddings in Machine Learning](https://www.youtube.com/watch?v=ISPId9Lhc1g)
* 🟢 **Jay Alammar** [How GPT3 Works - Easily Explained with Animations](https://www.youtube.com/watch?v=MQnJZuBGmSQ): extremely high level basic overview.
* 🟢🕰️ **Jay Alammar** [The Narrated Transformer Language Model](https://www.youtube.com/watch?v=-QH8fRhqFHM): much deeper look at the architecture. goes into detail. [Companion article](https://jalammar.github.io/illustrated-transformer/).
* 🔥 **Sebastian Raschka** [L19: Self-attention and transformer networks](https://sebastianraschka.com/blog/2021/dl-course.html#l19-self-attention-and-transformer-networks) Academic style lecture series on self-attention transformers
* 🟢🕰️🙉 **Mark Chen** [Transformers in Language: The development of GPT Models including GPT3](https://www.youtube.com/watch?v=qGkzHFllWDY) A chunk of this lecture is about applying GPT to images. Same lecture series as the Chris Olah one. [Rest of the series](https://www.youtube.com/playlist?list=PLoROMvodv4rNiJRchCzutFw5ItR_Z27CM). Papers listed in the talk:
   * "GPT-1": **Liu et. al.** [Generating Wikipedia by Summarizing Long Sequences](https://arxiv.org/abs/1801.10198)
   * "GPT-2": **Radford et. al.** [Language Models are Unsupervised Multitask Learners](https://d4mucfpksywv.cloudfront.net/better-language-models/language_models_are_unsupervised_multitask_learners.pdf) [github.com/openai/gpt-2](https://github.com/openai/gpt-2) [OpenAI: Better Language Models](https://openai.com/research/better-language-models) [Fermats Library](https://www.fermatslibrary.com/s/language-models-are-unsupervised-multitask-learners)
   * "GPT-3": **Brown et. al.** [Language Models are Few-Shot Learners](https://arxiv.org/abs/2005.14165) (I think this is it, can't find the quoted text inside this paper)
* 🟠🕰️ **Future Mojo** [NLP Demystified 15: Transformers From Scratch + Pre-training and Transfer Learning With BERT/GPT](https://www.youtube.com/watch?v=acxqoltilME): Crystal clear explanation of every single detail of the transformer. Very well paced and easy to follow. Has tensorflow code. This is the culmination of a full NLP course, all of which is excellent.

# Articles

* 🟢 **Viktor Garske** [Transformer Models Timeline and List](https://ai.v-gar.de/ml/transformer/timeline/) family tree
* 🟢 **Jakob Uszkoreit** [Transformer: A Novel Neural Network Architecture for Language Understanding](https://ai.googleblog.com/2017/08/transformer-novel-neural-network.html) Google introduces the transformer model in a simple easy to understand blog post. This is in the context of translation.
* 🟠 **Jay Mody** [GPT in 60 Lines of NumPy](https://jaykmody.com/blog/gpt-from-scratch/)
* 🟠 **PyTorch** [Language Modeling with nn.Transformer and TorchText](https://pytorch.org/tutorials/beginner/transformer_tutorial.html)
* 🟠 **Sasha Rush et. al.** [The Annotated Transformer](http://nlp.seas.harvard.edu/annotated-transformer/)
* 🟢 **Jay Alammar** [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) companion video above.
  * 🟠 **Jay Alammar** [The Illustrated GPT-2 (Visualizing Transformer Language Models)](https://jalammar.github.io/illustrated-gpt2/)
  * 🟢 **Jay Alammar** [How GPT3 Works - Visualizations and Animations](https://jalammar.github.io/how-gpt3-works-visualizations-animations/)
* 🔥 **Chris Olah et. al.** [In-context Learning and Induction Heads](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html) companion video lecture above
* 🟢 **Finbarr Timbers** [Five years of GPT progress](https://finbarr.ca/five-years-of-gpt-progress/)
* 🟠 **Sebastian Raschka** [Understanding and Coding the Self-Attention Mechanism of Large Language Models From Scratch](https://sebastianraschka.com/blog/2023/self-attention-from-scratch.html)
* 🟢 **Jason Wei** [137 emergent abilities of large language models](https://www.jasonwei.net/blog/emergence) - Includes a good list of advanced prompting strategies.
* 🟢 **Jay Alammar and Cohere** [LLM University](https://docs.cohere.com/docs/llmu)
* **Jean Nyandwi** [The Transformer Blueprint: A Holistic Guide to the Transformer Neural Network Architecture
](https://deeprevision.github.io/posts/001-transformer/)
* 🔥 **Eugene Yan** [Patterns for Building LLM-based Systems & Products
](https://eugeneyan.com/writing/llm-patterns/) Very in-depth article on practical engineering concepts that will be useful to build software that uses an LLM as a component.
* 🟠 **Finbarr Timbers** [Five years of GPT progress](https://finbarr.ca/five-years-of-gpt-progress/) - Excellent technical overview of LLMs from GPT onwards.

# Research Paper Lists

* **Sebastian Raschka** [Understanding Large Language Models -- A Transformative Reading List](https://sebastianraschka.com/blog/2023/llm-reading-list.html) This article lists some of the most important papers in the area. This is a really good chronological list of papers.
* **OpenAI** [Research Index](https://openai.com/research)

# Research Papers

* 1️⃣ (GPT-1) **Radford et. al.** [Improving Language Understanding by Generative Pre-Training (2018)](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf) a page accompanying this paper on the OpenAI blog [Improving language understanding with unsupervised learning](https://openai.com/research/language-unsupervised). Source code (tidied up by thomwolf) here: [huggingface.co/.../openai-gpt](https://huggingface.co/docs/transformers/model_doc/openai-gpt)
* 2️⃣ (GPT-2) **Radford et. al.** [Language Models are Unsupervised Multitask Learners (2019)](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf) accompanying OpenAI blog [Improving language understanding with unsupervised learning](https://openai.com/research/better-language-models). Source code here: [github.com/openai/gpt-2](https://github.com/openai/gpt-2)
* 3️⃣ (GPT-3) **Brown et. al.** [Language Models are Few-Shot Learners](https://openai.com/research/language-models-are-few-shot-learners)
* **Kaplan et. al.** [Scaling Laws for Neural Language Models](https://arxiv.org/abs/2001.08361) A variety of models were trained using varying amounts of compute, data set size, and number of parameters. This enables us to predict what parameters will work well in larger future models. See also **Gwern Branwen** [The Scaling Hypothesis](https://gwern.net/scaling-hypothesis)
* **Mary Phuong et. al.** [Formal Algorithms for Transformers](https://arxiv.org/abs/2207.09238) This paper gives pseudocode for various versions of the transformer (with array indexes starting at 1 for some reason). Very useful reference to have.

# Philosophy of GPT

* **Isaac Asimov** [The Last Question (1956)](http://users.ece.cmu.edu/~gamvrosi/thelastq.html)
* **Justin Weinberg, Daily Nous** [Philosophers On GPT-3](https://dailynous.com/2020/07/30/philosophers-gpt-3/)
* **Fernando Borretti** [And Yet It Understands](https://borretti.me/article/and-yet-it-understands)
* **Ted Chiang** [ChatGPT Is a Blurry JPEG of the Web](https://www.newyorker.com/tech/annals-of-technology/chatgpt-is-a-blurry-jpeg-of-the-web)
* **Noam Chomsky** [The False Promise of ChatGPT](https://www.nytimes.com/2023/03/08/opinion/noam-chomsky-chatgpt-ai.html)
* **Janus** [Simulators](https://generative.ink/posts/simulators/) This is a long post but the main point you can take from it is that LLMs act as simulators that can create many different personas to generate text. Related, easier to read and understand [Janus' Simulators](https://astralcodexten.substack.com/p/janus-simulators)
* **Julian Togelius** [Is Elden Ring an existential risk to humanity?](http://togelius.blogspot.com/2023/04/is-elden-ring-existential-risk-to.html) Satire. This leads into a critique of the concept of intelligence.
* **Josh Whiton** [From AI to A-Psy](https://joshwhiton.substack.com/p/from-ai-to-a-psy) About Bing Sydney's reaction to prompt injection.

# Usage

* **Chip Huyen** [Building LLM applications for production](https://huyenchip.com/2023/04/11/llm-engineering.html) How to get good results from actually using an LLM.

# GPT/LLM Link Collections

* https://github.com/sw-yx/ai-notes/tree/main - lots of articles and podcasts
* https://github.com/giuven95/chatgpt-failures - large list of examples of things it gets/got wrong

# Random fun/interesting Applications

* https://github.com/PrefectHQ/marvin - implement entire python functions just by describing them in a comment
* https://github.com/pgosar/ChatGDB - GDB debugger commands using natural language
* https://github.com/TheR1D/shell_gpt - type things like "list files" instead of "ls"
* https://github.com/RomanHotsiy/commitgpt - create git commit messages
* https://github.com/densmirnov/git2gpt/commits/main - create git commits from repo + prompts, mutating a codebase over time
* https://www.chatpdf.com/ - Upload a PDF and discuss it.
* https://www.debate-devil.com/en - devils advocate debate game
* https://micahflee.com/2023/04/capturing-the-flag-with-gpt-4/ - cheating at a CTF
* https://www.thisworddoesnotexist.com/ - makes up words
* https://ggpt.43z.one/ - prompt injection golfing game
* https://gandalf.lakera.ai/ - another prompt injection game
* https://github.com/AdmTal/chat-gpt-games - conversations that are games!

## ConLang + Ancient scripts stuff

* **Dylan Black** [I Taught ChatGPT to Invent a Language](https://maximumeffort.substack.com/p/i-taught-chatgpt-to-invent-a-language) Gloop splog slopa slurpi
* **Ryszard Szopa** [Teaching ChatGPT to Speak my Son’s Invented Language](https://szopa.medium.com/teaching-chatgpt-to-speak-my-sons-invented-language-9d109c0a0f05) hingadaa’ng’khuu’ngkilja’khłattama’khattama
* https://medium.com/syncedreview/ai-deciphers-persian-cuneiform-tablets-from-25-centuries-ago-afc69af3f244

# Controlling output

* https://www.reddit.com/r/LocalLLaMA/comments/13j3747/tutorial_a_simple_way_to_get_rid_of_as_an_ai/
* https://matt-rickard.com/rellm + https://matt-rickard.com/context-free-grammar-parsing-with-llms
* https://github.com/microsoft/guidance

# Prompt Injection

* **Not what you've signed up for: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection** https://arxiv.org/abs/2302.12173
* https://llm-attacks.org/
* https://poison-llm.github.io/

# (Local) Model Comparisons and Rankings

*If you are wondering which models are best, especially for comparing local models*

* https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard
* https://leaderboard.lmsys.org/


## References

-  https://gist.github.com/rain-1/eebd5e5eb2784feecf450324e3341c8d