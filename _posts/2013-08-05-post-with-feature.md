---
layout: post
title: Visualizing passages of music
description: null
category: articles
tags:
  - intro
  - beginner
  - jekyll
  - tutorial
image:
  feature: 'https://farm8.staticflickr.com/7014/6840341477_e49612bc59_o_d.jpg'
published: true
---

“Whenever you feel like criticizing any one,” he told me, “just remember that all the people in this world haven’t had the advantages that you’ve had.”

```python
import essentia 
from essentia.standard import *
loader = essentia.standard.MonoLoader(filename = 'LaxmibaiBahar.mp3', sampleRate = 44100)
audio=loader()
```

```python
pExt = PredominantPitchMelodia(frameSize = 2048, hopSize = 128)
```

```python
pitch, pitchConf = pExt(audio)
```

```python
time=numpy.linspace(0.0,len(audio)/44100.0,len(pitch))
output = np.column_stack((time.flatten(),pitch.flatten()))
np.savetxt('LaxmibaiBahar.txt',output,delimiter='\t')
```

He didn’t say any more, but we’ve always been unusually communicative in a reserved way, and I understood that he meant a great deal more than that. In consequence, I’m inclined to reserve all judgments, a habit that has opened up many curious natures to me and also made me the victim of not a few veteran bores. The abnormal mind is quick to detect and attach itself to this quality when it appears in a normal person, and so it came about that in college I was unjustly accused of being a politician, because I was privy to the secret griefs of wild, unknown men. Most of the confidences were unsought — frequently I have feigned sleep, preoccupation, or a hostile levity when I realized by some unmistakable sign that an intimate revelation was quivering on the horizon; for the intimate revelations of young men, or at least the terms in which they express them, are usually plagiaristic and marred by obvious suppressions. Reserving judgments is a matter of infinite hope. I am still a little afraid of missing something if I forget that, as my father snobbishly suggested, and I snobbishly repeat, a sense of the fundamental decencies is parcelled out unequally at birth.
