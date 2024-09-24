# How English Leaners Think

As an immigrant who moved to the United States at the age of 23, I have been struggling with learning English as my second language. I often complain that it is too old to learn a new language. My mindset has been fixed, my mouth muscles can hardly altered, and the gap between my peers and me is irreversibly large. 

Almost no one loves hearing complaining, but I am an exception. I can feel the depth of thinking from people's complaints, as what people store in their hearts for a long time is digested and trimmed innumerable times, likening to jade carved countless times. If you are like me, you must be curious about how English learners think when they use the language, and these Q&As may give you a hint.

## Why Are Their Sentences So Strange?

Every language has a cradle, the place where it was born and developed. The closer the cradle of the two languages, the more similar they are. There are many languages in Europe, but most have lots of similarities, such as alphabet, grammatical features, and scientific terminologies. You can easily infer that the difference between English and French is much smaller than the difference between English and Arabic or Japanese. 

As a dominant country with an extensive history, China has a couple of languages as well. However, most are regarded as dialects since they are all subject to the Chinese writing system to some extent. Cantonese is a well-known Chinese dialect. It became celebrated because it is the official language of Hong Kong, where the economy was streets ahead among Asian countries when it was under British rule. As a bilingual in Mandarin and Cantonese, I have learned considerably more expressions, idioms, and cultural references, which later encouraged me to learn other dialects and accents all around the country.

I can perceive the great difference between Chinese, Cantonese, and English. It turns out that the difference between Mandarin and Cantonese and the difference between Mandarin and English are not in the same order of magnitude. Before I get increasingly verbose, I would like to introduce some denotations to better explain the concepts.

We denote the difference between the two languages as $\text{DIFF}(X, Y)$, where $X$ and $Y$ are two different languages. It is difficult to quantify this value, but I came up with one the other day on my way to Burger King. Let's use lowercase letters to represent sentences. If a sentence x is written in the language $X$, we denote it as $x \in X$. We can express a sentence $y$ as its translation into another sentence $x$, denoted by the function $T(x)$, such that $y=T(x)$. Here, $T(x)$ is called the translation function. Note that $x$ and $y$ may not belong to different languages, and I will explain it later.

Before we delve deeper, let's recall our elementary school essays, and compare them to our current writing. Even though you and your elementary school self want to express the same idea, you tend to write in advanced sentences, as you know much vocabulary and have mastered more writing skills. Suppose you and your elementary school self are requested to write one sentence to describe your house, and you two write $x$ and $y$ respectively. Your teacher can tell the difference between them and give both sentences scores. Denote $\text{diff}(x,y)$ as the difference between the two sentences.

If you are asked to improve your essays in elementary school, you can rephrase them and composite better ones. I call the process "Intralingual Translation". When you converse with an intermediate-level English learner, you must find their sentences are a bit weird, but still you can understand. You unconsciously translate these sentences to what you are more familiar with. Sometimes you may speak it out by starting with "Do you mean", and the learner fortunately learns a precious idiomatic expression. In this case, you are doing intralingual translation.

Let's get to the point. We have two languages $X$ and $Y$, and a sentence $x_1 \in X$, which follows language conventions. We first translate $x_1$ into $Y$: $y_1 = T_1(x_1)$, where $y_1 \in Y$ and $T_1$ translates sentences word-for-word. Next, translate $y_1$ into $y_2$: $y_2 = T_2(y_1)$, where $T_2$ is an intralingual function, which rephrases a sentence into an idiomatic form. Subsequently, translate $y_2$ back into $X$: $x_2 = T_3(y_2)$, where $T_3$ translates sentences word-for-word as $T_1$. Finally, find the difference between $x_1$ and $x_2$. The summation of the differences of a great deal of samples can be used to estimate the difference between two languages. Since the nesting relationship of $T_1$, $T_2$, $T_3$ is fixed, we denote $T_r(x)=T_3(T_2(T_1(x)))$, and we obtain

$$
\text{DIFF}(X, Y) = \sum^{\infty}{\text{diff}(x_i, T_r(x_i))}
$$
To express this equation in natural language, the difference between two languages $X$ and $Y$, is determined by the summation of the difference between any given sentence $x_i$, which is written in language $X$, and its counterpart $T_r(x_i)$.
## Home Advantage

I am not able to count the time