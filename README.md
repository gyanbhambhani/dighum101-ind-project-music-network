# Mapping the Internal Structure of Music

## The idea

There is a digital humanities project from Northwestern called Visualizing Les
Miserables. It takes one novel and turns it into a network. The rule is simple:
if two characters appear in the same scene, draw a line between them. Once you do
that for every scene, the lines add up into a map of the whole book. You can see
who is central, who is on the margins, and which characters form tight groups.
The map shows you something true about the novel that is hard to notice while
reading it page by page.

This project asks whether the same move works on a piece of music. A score is
also a structured cultural text. It has parts that sound together, come back, and
build up an internal shape that performers feel but rarely get to see laid out. I
play in orchestras across classical and mariachi traditions, so I carry that
sense of structure in my hands and ears. The goal here is to make it visible and
to read the result back against what I already know as a player.

## What counts as a "moment"

In a novel a scene is obvious. In music a moment is fuzzy. It could be an
instant, a beat, or a whole phrase. So the heart of this project is committing to
a precise rule and then being honest about what that choice does to the result.

I build two separate networks, each with one clear kind of node and one clear
rule for edges.

**Pitch-class network (the main one).** The nodes are the twelve pitch classes
(C, C#, D, and so on up to B). Octaves are collapsed, so every C is the same
node. I cut the piece into quarter-note beats and connect any two pitch classes
that sound in the same beat. The more often a pair sounds together, the heavier
the edge. This keeps the graph small enough to read while still exposing the
harmonic backbone of the piece.

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
   Bach versus mariachi?

## The data

The corpus is J.S. Bach's four-voice chorales, which ship inside the music21
library as clean encoded scores. I start with one chorale (BWV 66.6) for close
reading, then scale the same code up to a batch of chorales so I can look at the
shared structure of the style. The cross-tradition comparison with mariachi or
folk music is left as future work, since it needs clean symbolic files that I do
not have yet. The pipeline already accepts any MIDI or MusicXML file, so adding
that later is a matter of dropping files into `data/`.

## What is in the notebook

The notebook walks through the whole argument in order:

1. Define the nodes and edges precisely.
2. Load a chorale and detect its key.
3. Build the pitch-class network for one piece.
4. Rank pitch classes by centrality (question 1).
5. Detect communities and name them as chords (question 2).
6. Draw the network, including a circle-of-fifths layout that makes harmony
   visible.
7. Compare a beat window against a measure window.
8. Build the instrument network.
9. Scale up to many chorales and aggregate them.
10. Prune the dense aggregate so it stays readable.
11. Write up findings and limitations.

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
between the network and my ear. Where the network confirms what I expect, like
the dominant turning out to be the most central note in a minor key, it tells me
the structure is real and something I can point to, instead of just a feeling.
Where it disagrees or surprises me is where the interpretation gets interesting,
and that is the actual work.

## Running it

```bash
python3 -m venv dighum101
source dighum101/bin/activate          # Windows: dighum101\Scripts\activate
pip install -r requirements.txt
jupyter notebook notebook.ipynb
```

Select the `dighum101` kernel and run the cells in order. Figures and tables are
written to `outputs/`.
