# Machine Reading

Open information extraction: Extract structured knowledge from unstructured texted without knowing what the entities or relations are in advance. 

This is unsupervised.

## Distant Supervision
We can get away from knowing fixed relationships. 
Instead of manually labeling from an existing knowledge base.

Positive Example: Every entity is related to another entity in a sentence. Jack loved his sister, Jill, very much. Jack and Jill are the entities connected by the relation, love. 

Negative Example: No relation between entities. Jack and Jill went up the hill.

Open Info Extraction requires learning a way to recognize relations without labeling. The **syntax** of sentences provide clues about relations.

Semantics arise from syntax. "Jane loves science." We get the meaning because we understand Jane is entity, science is a concept, and love is their positive affinity relationship.

Part of Speech (POS) tagging.
Assign a POS tag to each word in a sentence.
Dynamic  Bayesian Network:
Everything in the top row (tags) are latent properties of the document: they don't really exist.
If tag is VRB we tend to see certain words we recognize as verbs, so the relationship is an 'emission.' P(word|tag). If a certain tag is present then it emits certain words with greater probability. 
It's a dynamic bayesian network because we have this tag1 > tag2 relationship, sequentially. We tend to see certain things follow other tags. P(tag2|tag1)
Then we search for a set of tags that maximized the joint probability 
1. Time proceeding through network
tag1 >  tag2 >  tag3 
v       v       v
word1   word2   word3

Dynamic Programming:
We have words, not tags.
Compute most likely explanation MLE: What sequence of latent values would most likely emit the observed sequence of words. Use the trick of dynamic programming, assume that choosing most likely tag for each time slice results in the most likely explanation for the entire sequence.

Dependency Parsing
Words have relationships to each other
Graph-structured syntactic analysis of sentence that helps us find these relationships.
a. root is a verb
b arc are dependencies with types, telling us what is related and how.
c. shift-reduce parsing
1 build a dependency graph from the bottom up
2. Buffer: data structure: Processes a sentence word by word from a buffer
3. Stack: data structure: put words that we don't know what to do with in the stack.
4. Operations

But how do we know which operation (shift, left, or right?): Learn a classifier: supervised learning. 
Given: a stack, a buffer, and POS tags
Output: shift, left, right and maybe dependency type
Build the dataset:
1. distant supervision
2. annotate lots of sentences with dependency graphs/parses
manual process (people do it)
Run stack-reduced operation forward and backwards using different operations until it matches the annotations we put in.
This means backtracking when necessary to find the known graph
It predicts the operations we have to do to get the answers according humans
We end up with:
1 X: partial parses of sentences (buffer, stack, POS tags), different stages at each moment: Input to classifier.
2. Y: Best shift-reduce operations: labels for the classifier.

---
Dependency Parsing
Words have relationships to each other
Graph-structured syntactic analysis of sentence that helps us find these relationships.
a. root is a verb
b arc are dependencies with types, telling us what is related and how.
c. shift-reduce parsing
1 build a dependency graph from the bottom up
2. Buffer: data structure: Processes a sentence word by word from a buffer
3. Stack: data structure: put words that we don't know what to do with in the stack.
4. Operations

But how do we know which operation (shift, left, or right?): Learn a classifier: supervised learning. 
Given: a stack, a buffer, and POS tags
Output: shift, left, right and maybe dependency type
Build the dataset:
1. distant supervision
2. annotate lots of sentences with dependency graphs/parses
manual process (people do it)
Run stack-reduced operation forward and backwards using different operations until it matches the annotations we put in.
This means backtracking when necessary to find the known graph
It predicts the operations we have to do to get the answers according humans
We end up with:
1 X: partial parses of sentences (buffer, stack, POS tags), different stages at each moment: Input to classifier.
2. Y: Best shift-reduce operations: labels for the classifier.

__

Dependency Parsing and Distant Supervision

Goal: 

Analyze the grammatical structure of a sentence by building a dependency graph: a tree where

The root is usually a verb

Arcs are labeled with dependency types (e.g., subject, object) showing how words relate.

How do we build the dependency graph:

We use a shift-reduce parser, which processes the sentence bottom-up using:

A buffer: words we still need to process

A stack: words we've seen but haven't fully connected yet

Operations (shift, left-arc, right-arc): actions that modify the stack and buffer and build the graph

The problem: How we we know which operation to perform at each step?

Solution:
Train a classifier using supervised learning.

Building the Training Dataset: Distant Supervision
Since we need training examples of (stack, buffer, POS tags) mapped to best action, we simulate the parsing process using sentences already annotated with correct dependency trees (i.e., ground truth graphs).

Steps:

Manually annotated sentences:
Humans create gold-standard dependency parses for lots of sentences. These are the raw material for the training data, but not the final form of the training example the classifier sees directly

Replay parsing (shift-reduce simulation):
We simulate the parsing process forward, trying operations (shift, left-arc, right-arc).

Backtracking if necessary:
If a wrong operation is chosen, we backtrack and adjust to make sure the sequence of operations leads exactly to the human-annotated tree.

# Create training examples:

X (input): Current parser state (stack, buffer, POS tags at that moment)

Y (label): The correct next action (shift, left-arc, right-arc, with possibly a dependency label)

# Why is this called "distant supervision"?

We aren't labeling operations directly (no one manually labeled "shift here", "left-arc here").

Instead, we use the distant information (full annotated trees) and infer the correct operations needed to build them.

Supervision is based on final desired output, not step-by-step annotations.

In short:
We use final human-labeled parses to generate supervision over many intermediate parsing states.

Given gold parse:
"She eats apples" → "eats" is root, "She" is subject of "eats", "apples" is object of "eats."

The simulation will do (each of the following three is a training example):

Stack: [], Buffer: [She, eats, apples] → Action: shift

Stack: [She], Buffer: [eats, apples] → Action: shift

Stack: [She, eats], Buffer: [apples] → Action: left-arc (nsubj)

and so on...

Each (stack, buffer, POS) → action pair becomes a training sample.

The original graph tells us what the right action should be at each step — but the classifier never gets the full graph as input.

Instead of making the model predict an entire tree at once (too hard),

We teach it moment-by-moment what the best move is,

Based on the gold dependency tree created by humans.
# Example

Suppose sentence:
"She eats apples."

Gold parse says:

"eats" is root

"She" → subject of "eats"

"apples" → object of "eats"

Training samples would look like:


Partial state (stack, buffer, POS)	Best action (label)
Stack: [], Buffer: [She, eats, apples], POS: [...]	shift
Stack: [She], Buffer: [eats, apples], POS: [...]	shift
Stack: [She, eats], Buffer: [apples], POS: [...]	left-arc (subject)
Stack: [eats], Buffer: [apples], POS: [...]	shift
Stack: [eats, apples], Buffer: [], POS: [...]	right-arc (object)

We train on (parser state → next action) pairs.
We build the full graph by stringing together many next actions.
The original annotated dependency graph is used only to supervise, not fed in directly.

---

During Inference:
We have a new sentence.

Start:

Stack is empty []

Buffer has all the words of the new sentence [The, cat, eats, fish]

POS tags are already assigned (by a separate POS-tagger)

At each step:

The parser looks at the current stack + buffer (+ POS tags)

Uses the trained classifier to predict the best action: shift, left-arc, or right-arc (and maybe dependency label)

Apply the predicted action:

If classifier says shift: move word from buffer to stack

If left-arc: create an arc and pop the second-to-top word from the stack

If right-arc: create an arc and pop the top word from the stack

Repeat:

Keep feeding the new parser state into the model

Keep taking actions

Until the buffer is empty and only one item remains on the stack (the root)

Result:

You have a **dependency graph** for the whole sentence!

By dependency parsing the arc map we can make tuples from the unstructured sentences <subject, relation, object>, because it walks us through the graph of the sentence.

Put these tuples in the knowledge base to tell us the different ways the entities are related to each other. This makes a structured database.

If we don't have perfect matches we use distributional approaches to match questions to relations. Cosine similarity helps us decide if this is the best tuple: we use cosine similarity between different parts of the sentence and pieces of info we have in the tuple.

Parse a sentence → build a dependency graph (using your shift-reduce parser).

Walk the dependency graph to extract meaningful (subject, relation, object) triples:

Subject = the entity doing the action

Relation = the action or link

Object = the entity receiving the action

→ Example:
Sentence: "The cat eats fish."
Dependency graph shows:

cat (subject) → eats (verb)

eats → fish (object)
Tuple extracted: (cat, eats, fish)

Store the triples in a knowledge base:

Now you have structured, machine-readable data!

Entities are related by known relationships.

Natural language is messy: different ways to say the same thing.

Distributional approaches help:

Use word embeddings (like Word2Vec, GloVe) to represent words or phrases as vectors.

Use cosine similarity between:

Words in the question

Words in the knowledge base tuples

This lets you approximate matches, even if the phrasing isn't exactly the same.

Example:

Question: "What does a feline consume?"

Knowledge base: (cat, eats, fish)

Even though "feline" ≠ "cat" and "consume" ≠ "eats," cosine similarity would reveal they're close!

---
That works for who what where and when questions. But why questions are harder.

Events are descriptions of changes in the state of world (actual or implied). We need to get at the semantics of the events because these are not in the text.

Frames:
Acknowledge that a word's meaning cannot be understood without access to essential knowledge that relates to the world.

A Frame is a system that relates concepts such that understanding one concept requires the understanding of all concepts and their relationships

circular: seller > sell > recipient > possession > seller ...

FrameNet: avenger, punishment, offender, injury, injured-party
Made by humans
Lexical units can bring up this frame: get back at, get even, avenge...

If we can identify the frame and the frame elements, we can make inferences about the relationships between the frame elements that we might not be able to do from just the surface form of the sentence.

Sally wanted to get back at Jane after her cat died.
Why did sally want to do this?
"We need to understand 'get back at'.

VerbNet: Linguist resource of frames centered around verbs.

Links syntactic and semantic patterns.

Can be used for:
Word Sense Disambiguation 
Figurative Language Detection

VerbNet organizes verbs into classes based on their meaning and syntax.

Each verb class defines:

What kinds of participants (roles) it expects (Agent, Patient, Instrument, etc.)

What kinds of syntactic frames are typical (subject-verb-object, subject-verb, etc.)

It also gives semantic roles and predicates that describe the event structure clearly.

 Key: VerbNet links natural language to structured events and roles.