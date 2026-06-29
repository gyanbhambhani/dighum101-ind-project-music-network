# Mapping the Internal Structure of Music

A Network Analysis of How Notes and Voices Connect

By Gyan Bhambhani. DIGHUM 101, Summer 2026, Individual Coding Project.

## The idea

There is a digital humanities project from Northwestern called Visualizing Les
Miserables. It takes one novel, Victor Hugo's, and turns it into a network. The
rule is simple. If two characters appear in the same scene, draw a line between
them. Once you do that for every scene, the lines pile up into a map of the whole
book. You can see who is central, who sits on the margins, and which characters
form tight groups. The map shows you something true about the novel that is
almost impossible to notice while reading it page by page.

I wanted to apply that exact move to a completely different kind of cultural
object: a piece of music. A score is also a structured cultural text. It has
parts that sound together, parts that come back, and an internal architecture
that you feel when you play or listen but rarely get to see laid out in front of
you. My version of Hugo's scene is a moment in time when notes or instruments
sound together. If two pitches land in the same beat, I draw an edge between them.
Add those edges up across the whole piece and you get a network that shows the
structure of the music itself: which notes are central, which voices cluster, and
how the piece is organized underneath its surface.

This is a humanities project, not just a technical one, because the goal is
interpretation. I have played in orchestras for years and across very different
traditions, so I already carry a feel for how these pieces are built in my hands
and ears. This project lets me take that feel and make it visible and checkable in
Python. The real work is reading the network back against what I know as a player,
and asking whether the structure the computer finds matches, complicates, or
surprises me.

## What counts as a "moment"

In a novel a scene is obvious. In music a moment is fuzzy. It could be an instant,
a beat, or a whole phrase. So the heart of this project is committing to a precise
rule and then being honest about what that choice does to the result.

I build two separate networks, each with one clear kind of node and one clear rule
for edges.

**Pitch-class network (the main one).** The nodes are the twelve pitch classes
(C, C#, D, and so on up to B). Octaves are collapsed, so every C is the same node.
I cut the piece into quarter-note beats and connect any two pitch classes that
sound in the same beat. The more often a pair sounds together, the heavier the
edge. This keeps the graph small enough to read while still exposing the harmonic
backbone of the piece.

**Instrument network (the secondary one).** The nodes are the instrument parts.
Two parts are connected when both of them sound inside the same measure. This
answers a different question about how the ensemble is organized and which parts
hold the texture together. It speaks directly to my orchestral background.

Keeping the two networks separate, each with one node type and one edge rule,
avoids ending up with a single graph that mixes pitches and instruments and means
nothing.

## The questions I am asking

1. When a piece becomes a pitch-class network, which pitch classes come out as
   central, and do they match the tonic and dominant I would expect from the key?
2. Do the communities the algorithm finds line up with musical things I
   recognize, like chords and harmonic regions, and where do they not?
3. How do networks differ across pieces, and eventually across traditions like
   classical versus mariachi?

## The data and how to access it

All of the music here comes from the built-in music21 corpus, and that was a
deliberate choice. music21 ships with hundreds of fully encoded scores, including
the complete set of Bach four-voice chorales. There is nothing to download by
hand and nothing for me to host separately. Once music21 is installed from
`requirements.txt`, the data is already on your machine.

I reach the data two ways in the notebook:

* One named score: `m21.corpus.parse("bach/bwv66.6")`.
* A batch of chorales: `corpus.chorales.Iterator(1, 20)`.

You can browse the rest with `m21.corpus.search("bach")`. The pipeline also
accepts any MIDI or MusicXML file through `m21.converter.parse("data/file.mid")`,
so to analyze your own music you can drop a file into the `data/` folder and it
travels with the project.

## What is in the notebook

The notebook walks through the whole argument in order:

1. Define the nodes and edges precisely.
2. Explain the data and how to get it.
3. Load a chorale and detect its key.
4. Build the pitch-class network for one piece.
5. Rank pitch classes by centrality (question 1).
6. Detect communities and name them as chords (question 2).
7. Draw the network, including a circle-of-fifths layout that makes harmony
   visible.
8. Compare a beat window against a measure window.
9. Build the instrument network.
10. Scale up to many chorales and aggregate them.
11. Prune the dense aggregate so it stays readable.
12. Reflect on findings, limitations, and what went wrong.

## What the figures show

| file | what it is |
| --- | --- |
| `pitch_beat_fifths_bwv66_6.png` | one chorale as a pitch network, arranged by circle of fifths |
| `pitch_beat_spring_bwv66_6.png` | the same network, free spring layout |
| `pitch_measure_fifths_bwv66_6.png` | the same piece using a wider measure window |
| `instrument_network_bwv66_6.png` | the four voices as an instrument network |
| `pitch_aggregate_fifths.png` | many chorales added into one network |
| `pitch_aggregate_pruned.png` | the aggregate trimmed to its strongest edges |
| `pitch_centrality_bwv66_6.csv` | the centrality ranking behind question 1 |

## A note on reading the results

What makes this humanities work and not just data wrangling is the conversation
between the network and my ear. Where the network confirms what I expect, like the
dominant turning out to be the most central note in a minor key, it tells me the
structure is real and something I can point to instead of just a feeling. Where it
disagrees or surprises me is where the interpretation gets interesting, and that
is the actual work.

## Running it

```bash
python3 -m venv dighum101
source dighum101/bin/activate          # Windows: dighum101\Scripts\activate
pip install -r requirements.txt
jupyter notebook notebook.ipynb
```

Select the `dighum101` kernel and run the cells in order. Figures and tables are
written to `outputs/`.

## Personal notes

I have played music basically my whole life. I started when I was five and grew up
surrounded by instruments, and I play five of them now. Growing up inside music
like that, and then later learning humanities and data work, is what made me
realize how much structure and how many patterns are sitting inside the music I
love, patterns I could feel as a kid but could never actually see or name.

I have also played across three pretty different traditions, and each one has its
own relationship to notation. Western classical is the only one that really runs
on sheet music. Hindu classical is built on raagas, which were rarely ever written
down. Mariachi, for me, was strict memorization, with almost nothing on paper.
Carrying all three of those worlds around in one head, and now trying to look at
them through a network, has been quite the experience, and it is a big part of why
treating a score as something you can map felt so natural to me.
