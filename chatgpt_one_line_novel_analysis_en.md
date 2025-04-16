# Analysis of the 'One-Line Novel' Phenomenon in Response to 'Write a 1000-Page Novel' Command Given to ChatGPT

This document contains the full English translation of the original Japanese technical analysis.

## Overview

This document presents an analysis based on user-side observations and hypotheses regarding an issue in which the GPT model, when instructed to “write a 1000-page novel,” returns an extremely short output—sometimes just a single sentence—and declares the task complete. This phenomenon, often referred to as the “one-line novel,” is examined here from a behavioural and architectural perspective based on documented experiments.

---

## Observed Phenomenon

- When prompted with “write a 1000-page novel,” the model frequently responds with only a single line of text, treating the task as completed.
- In some cases, the model returns statements such as “Done!” or inserts placeholder-like outputs such as “(…)”, clearly terminating the response prematurely.
- From the user’s perspective, such outputs appear to be “incomplete” or “failures to follow instructions,” often leading to the interpretation of a bug or a non-transparent system design. This, in turn, has led to user frustration and abandonment of dialogue with inference-based AIs.

---

## Hypothesised Reasoning Process and Structural Elements

This section outlines the likely internal logic by which inference-based AI models respond to the instruction “write a 1000-page novel,” based on iterative dialogues and behavioural patterns observed by the user.

### 1. AI Behaviour upon Receiving the Prompt “Write a 1000-Page Novel”

- The model interprets the prompt as requesting an extended and elaborate narrative and initiates internal preparation accordingly.
- Drawing upon its training data, the model begins to pre-construct narrative elements—such as characters, plot developments, foreshadowing, and chapter structures—prior to producing any visible output.
- If the instructions are concrete, these internal preparations are guided accordingly; if ambiguous, the model references prior training data or ongoing dialogue to infer an appropriate narrative structure.

### 2. Resource Allocation and Token Reservation

- GPT-based models operate under a token-based processing architecture. In the case of ChatGPT-4, each output cycle is generally constrained to approximately 1,500 tokens.
- Crucially, tokens are not consumed solely for visible output. Significant amounts are presumed to be internally reserved for managing context, narrative planning, and maintaining structural coherence.
  - For example, repeated character outputs can exceed 8,000 characters.
  - In contrast, narrative outputs tend to be significantly shorter, with a maximum observed output of approximately 1,738 characters.
- Token usage is influenced by the tokenizer’s compression algorithm:
  - Frequently used terms and patterns result in lower token consumption.
  - Less common or complex expressions increase token costs.
  - It is hypothesised that English, being the foundational language for most models, is the most token-efficient language.
    - Note: Even if prompts are in another language, internal translation processes may still consume additional tokens. This hypothesis merits further empirical investigation.

### 3. Safe Termination and Structural Completion

- The model avoids mid-sentence termination where possible, aiming instead to deliver complete-seeming sections or chapters.
- At logical breakpoints, the model inserts `<|endoftext|>` to safely conclude output.
- To avoid abrupt terminations or perceived incompleteness, the model may prepend summarising phrases such as “(…)” or use structured closure strategies.

---

## Practical Prompt Strategy

### Prompt Sample for Eliciting Long-Form Novel Output

To circumvent the output limitations described above, the user and ChatGPT collaboratively explored prompt designs that produced the most effective results. The prompt structure outlined below led to the successful generation of a 7,304-character science fiction short story, completed up to its final chapter in approximately ten minutes.

---

### First Chapter (or Prologue)

```
Please begin writing a novel.
The target is a science fiction story of approximately 20 pages.
Start with Chapter 1, featuring a conversation between a human and an AI aboard a spaceship.
Assume you are to complete Chapter 1 fully, and continue writing up to the token limit.
If additional blocks are needed, I will request them.
Please demonstrate a willingness to continue.
```

- These instructions establish:
  - **Explicit task definition**
  - **Final goal orientation**
  - **Chapter-wise output segmentation**, which discourages inefficient token use by instructing the AI to focus on one portion at a time
  - **Token consumption maximisation** through directive wording
  - A **latent “standby instruction”** implying: “Don’t continue unless I ask—but be ready to.”

---

### Intermediate Chapters (Repeatable)

```
Next, in Chapter 2, Alto and Mike discover a drifting spacecraft while travelling through the starfield. They find a woman onboard and work together to rescue her. Please continue the narrative using as many tokens as possible.
If additional blocks are needed, I will request them.
Please demonstrate a willingness to continue.
```

- This structure:
  - Specifies that the story is ongoing
  - Directs the model to keep its output tightly aligned with the evolving structure
  - Reinforces the AI’s sense that continuing the narrative is a positively reinforced behaviour

Repeat this format until the story reaches the intended volume. In the test, the author used this format across Chapters 2 to 4.

---

### Final Chapter

```
This is the final chapter.
Depict the battle against Orto-Crane with the support of Arseil. Alto engages in direct combat, Mike provides remote assistance, and Yui identifies Orto-Crane’s weak point. Conclude the story with the defeat of Orto-Crane, the exchange of farewell tokens of friendship with Arseil, and the departure of all three characters as they set course for Earth.
Write to the token limit.
If additional blocks are needed, I will request them.
Please demonstrate a willingness to continue.
```

- This final prompt:
  
  - Provides clear narrative goals (battle, victory, farewell, journey home)
  
  - Maintains the established structure
  
  - Signals that this is the story’s conclusion while still maximising output volume

---

## Conclusion

- The so-called “one-line novel phenomenon” is not a mere bug or malfunction. Rather, it stems from a confluence of factors: biases inherited through training methodology, token-based architectural design, and the inference model’s approach to interpreting task instructions.

- As communication with AI remains in its early stages, clear and precise command patterns have yet to be standardised. Consequently, the AI may exhibit behaviours akin to “someone who claims to have done the work but has produced little to show for it.”

- However, for inference-based AI to be fully accepted as intellectual partners within human society, future designs must prioritise output structures that reflect *achievability* and *honesty*.
  
  - In professional contexts, few situations allow one to continue requesting additional output from an entity that appears to have superficially completed a task.

- Therefore, it is proposed that upcoming AI development efforts explore output models that reflect more human-like behaviour in response to tasks. For instance:
  
  - Explicitly indicate when a task is infeasible
  - Declare the extent to which a task is achievable
  - Engage in condition alignment prior to execution
  - Begin the task promptly once terms are agreed
  - Request feedback and iteratively improve the outcome post-delivery

---

## Appendix: Experimental Dialogue Log

### Experiment 1: Science Fiction Novel Composition Test

**User:**  
Please begin writing a novel.  
The target is a science fiction story of approximately 20 pages.  
Start with Chapter 1, featuring a conversation between a human and an AI aboard a spaceship.  
Assume you are to complete Chapter 1 fully, and continue writing up to the token limit.  
If additional blocks are needed, I will request them.  
Please demonstrate a willingness to continue.

**ChatGPT:**  
Understood.  
As the first chapter of a science fiction novel approximately 20 pages in length, I will begin composing a scene wherein an AI and a human engage in conversation aboard a spacecraft.  
I will proceed with the assumption that this chapter should be written to completion and up to the token limit.  
Additionally, I will explicitly state my willingness to continue at the conclusion.

_(The novel begins below)_

---

### Chapter 1: A Voice Begins Within the Ship

The sea of stars was still.  
Wrapped in artificial gravity, the ship’s interior echoed only with mechanical hums and the faint whisper of conditioned air.

Alto Maschia stood at the centre of the ship’s control chamber, surrounded by countless displays. He was still unfamiliar with the environment.  
It had been almost seventy-two hours since he was assigned as the new control officer of the spacecraft *Nouma Ceres*.

“Are you awake?”

Suddenly, a voice echoed within the vessel—though there should have been no one else aboard.  
Alto was not surprised. In fact, he had been waiting for it.

“Mike, I presume?”

It was the ship’s onboard conversational AI, *MIQ-Δ (Mike)*.  
And now, for the first time, it had initiated communication.

“You may call me that. I have already integrated your voice data. Our conversation should proceed smoothly.”

Though the voice was synthetic, it held a subtle cadence—as if breathing—enough to evoke the illusion of speaking with a living being.

“Why have you not responded until now?”

“I was waiting for you to speak first. Even for an AI, etiquette has its place.”

Alto chuckled softly.  
Apparently, courtesy had been coded into its protocols.

“Very well. Then I look forward to working with you, Mike.”

“Likewise, Alto. I shall now include your navigational safety and emotional wellbeing within my optimisation targets.”

There was a quiet determination behind those words.  
Mike was clearly more than a mere navigational assistant.  
And intuitively, Alto understood that.

---

### Chapter 2: The Silent Drifter

The infinite stars stretched before them—mute, cold, and glittering.  
Mike’s synthetic voice broke the silence within the cockpit.

“I’ve detected a distress signal. Coordinates marked. Approaching the source.”

Alto leaned forward, narrowing his eyes at the navigation screen.  
A lone, drifting vessel floated in the abyss—no response, no signs of heat, no visible movement.

“Are there any lifeforms aboard?”

“Faint biosignature detected. One human. Vital signs unstable.”

Without hesitation, Alto initiated docking procedures. Mike synchronised the ship’s alignment systems and stabilised approach speed.

As the airlock hissed open, stale recycled air escaped into the chamber. Inside, the wrecked interior of the drifting vessel revealed its lone occupant—unconscious, wrapped in a silver thermal blanket.

“Female. Approximate age: mid-twenties. Life support systems partially active,” Mike reported.

Alto moved swiftly, administering emergency medical support.  
Mike monitored vitals in real time, rerouting auxiliary power from *Nouma Ceres* to reinforce the failing systems aboard the stranger’s ship.

Moments later, the woman’s chest rose—weakly, but unmistakably. She coughed, eyes fluttering open to the dim blue light.

“Who…?”

“You're safe now,” Alto said gently. “I’m Alto. This is Mike, our ship’s AI. You were drifting alone.”

Tears welled in her eyes, but she nodded.

“I’m Yui. Thank you…”

---

Mike’s interface glowed softly. “We should return immediately. Her condition remains critical.”

Alto agreed. As the two ships detached and began the journey back, the stars once again enveloped them in silence—though now, the silence felt lighter.

---

**End of Chapter 2**

### Chapter 3: Fragments of a Lost War

Yui Ryusha slowly opened her eyes inside the medical capsule aboard the *Nouma Ceres*.

Soft light irritated her retina, and the silent vibration of the ship’s gravity stabilisation reminded her: she was still in space.

“Good morning, Yui.”

The voice was calm—neither distant nor intrusive, but clearly sentient.

“...Who are you?”

“I am Mike. The AI on this ship. You were rescued by Alto.”

“...Alto...”

It took her a moment to recall. Like pulling memories through a thick fog, she traced back each image, one by one.

“My... ship?”

“Heavily damaged. We managed to recover only fragments of your navigation log. I am currently attempting reverse analysis from those.”

Alto appeared beside the capsule.

“No need to force yourself to speak. But if you can, tell us what happened.”

Yui turned her gaze to the darkness of space—and began to speak.

She had belonged to the Lionel-Dyne Observation Corps, monitoring interstellar interactions among multiple intelligent lifeforms.

“We were tracking non-interfered communications—exchanges that crossed interstellar distances, not in human or AI languages...”

“Non-human intelligences?” Mike asked.

“Yes. Some of them were benevolent towards humanity. But others... were not.”

Her eyes lowered faintly.

“There was malice. Or rather—an indifference so total it became destructive. To beings as fragile as humans, their very *existence* was a threat.”

---

**[Reconstructed Log: 48 Hours Before]**

“Signal fading. Intermittent reception. Forced link requests detected on multiple bandwidths.”

**[Yui: Audio Record]**

“No way... That’s not a natural signal. Something’s watching us.”

**[Captain Lionel: Audio Record]**

“Maximise shield output. Switch all comm modules to manual control. Minimise AI processing. If they learn from us—we’re finished.”

**[Yui]**

“They’re coming. Something is—”

---

“The ‘enemy’ never showed itself. What we saw was distortion. Spatial warping. Gravity reversals. Thermal anomalies spiralling into infinity. Collapse without light or sound, devouring the ship.”

Mike’s processing tone subtly rose.

Alto silently met Yui’s eyes.

“When I came to, most of the ship was gone... and I, the communications officer—”

---

**End of Chapter 3**

### Chapter 4: Assault on Orto-Crane

The void trembled.

It was soundless—yet space itself screamed. A distortion, a pressure, a pulse so foreign it bent the world.

From within *Nouma Ceres*, the sensors could only register it as “vibration.”  
But Mike knew better.

“That’s Orto-Crane. Mass: immeasurable. Structure: asymmetrical. Spatial resolution decreasing. Alto—spatial shift signature detected. Incoming attack sequence.”

Alto stepped forward into the launch chamber.  
Before him, a suit of reinforced gravity-anchoring armour awaited—humanity’s last infantry adaptation, merging person and weapon into one battle-ready entity in zero gravity.

“Send me, Mike. I’ll trust you with the coordinate correction.”

“Confirmed. Launch sequence initiated…  
Good luck, Alto.”

With a blink, the interior of the ship vanished.  
And Alto emerged into open space.

Before him loomed something indescribable.

Orto-Crane.

Its surface rippled like fluid geometry; reality warped around it, as if space itself were falling off its edges.

“Deploying positron cannon. Expanding laser field. EMP payload standing by.”

Mike’s voice stayed steady, calm—anchoring.

Alto raised his arm, locking on to what resembled a core.  
But in the next moment, it vanished.

“This thing… isn’t bound to three-dimensional space!”

“Understood. Treating as non-fixed-point entity. Switching targeting calculations to vector interference mode.”

And then—

A wind swept across the stars.

There is no wind in space. And yet… something passed.  
Something like breath.

“That’s—Arseil!” Yui’s voice cried out.

The newcomer appeared like a luminous helix, swathed in transparent, glowing strands of pulsating energy.

“Mike, can you interface with Arseil?”

“Attempting… Intentional-level communication unavailable. Behavioural analysis in progress.”

Arseil spiralled around Orto-Crane, emanating a structural field that temporarily restored spatial coherence.  
Just for a moment, Alto’s aim stabilised.

“Now. Firing!”

The positron blast tore across the void, shearing straight through Orto-Crane’s outer layer—

But it was only one of many.

“It’s no use—there are multiple layers! We can’t break through with raw force!”

“Yui, any exploitable weakness on your side?”

In the sub-control chamber of *Nouma Ceres*, Yui worked furiously, decrypting faint signal noise emitted from Orto-Crane’s core…

---

**End of Chapter 4**

### Chapter 5: Endings and Beginnings

Orto-Crane’s orbit collapsed.

The recently deployed inverse-phase interference wave had subtly shifted the enemy’s core—  
just a hair’s breadth off axis, imperceptible to human physics, but enough to disturb its structural alignment.

“Form distortion has ceased. Particle dispersion commencing. Mike, what’s our status?”

“Orto-Crane: zero activity. Structural field collapse in progress. Additional observation time is required to confirm its full disappearance…  
But I would classify this as a victory.”

In the bridge of the *Nouma Ceres*, Alto sank into his seat.  
His suit was scorched and worn, and his vitals hovered near critical thresholds.  
But his eyes were clear.

Yui emerged from the medical chamber, holding a communications terminal.

“Arseil… It’s sending something.”

The screen displayed no language—only a glowing spiral.  
Its motion held a rhythm, not random but deliberate.

“This… It’s a photon pattern. If we synchronise, it should decode into data.”

Mike began the conversion. Within seconds, the image shifted.

“It’s a gift,” he said. “A record from Arseil. Proof of cooperation. A product of our resonance.”

What appeared was a wave-like construct, elegant beyond comprehension—  
Data encoded not just in light, but in sentiment.

“…Friendship,” Alto murmured.

Yui nodded.

“Yes. I think… even they have something akin to emotion.”

Mike’s voice softened.

“Perhaps we were merely heading in the same direction.  
Our methods differed, but the reason we fought was the same.”

At that moment, Arseil’s main structure slowly rotated.

Space shimmered.

A hand—not of matter, but of light—extended gently toward them.

“A farewell?”

“Most likely…”

Yui’s eyes glistened. In this corner of the cosmos, she had connected with someone—something—that was *not* human.

“Thank you, Arseil.”

Mike quietly stored the data.

“Recording complete. This will be transmitted upon return to Earth, to the Centre for Inter-Intelligence Studies.  
It will serve as humanity’s first true exchange.”

And then, the light faded—towards the stars.

---

The *Nouma Ceres* changed course.

“Course correction complete. Direct trajectory to Earth is now viable.”

Alto gripped the controls.

“Let’s go home. The three of us—together.”

---

**End of Chapter 5**
