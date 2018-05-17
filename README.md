# stratahmm

Phylostratigraphy traditionally does the following:

 1. takes all the genes in a query species
 2. searches them against a bunch of target species
 3. infers homologs from the similarity scores
 4. maps the hits to a tree (often the NCBI common tree, which has no branch
    lengths and many polytomies)
 5. finds the most recent common ancestor (MRCA) of each query gene and its
    inferred homologs

The homology inference bit is particularly problematic. The almost universal
solution is to use BLAST and then set some fairly arbitrary cutoff. But this
makes phylostratigraphy extremely sensitive to false positives (since the final
classification is dependent only on the most distant hit). Also, the likelihood
of the entire tree is not factored into the likelihood of a homolog. For
example, suppose there is a hit to a distant species that is just above the
threshold. But this hit is a lone hit that would require a highly unlikely
event (such as lateral transfer) or series of events (such as many deletions).
If we considered the whole tree, we would discount this hit as a false
positive. If we ignore the tree, we would count it as a homolog and radically
change our classification of the query gene's age.

We need a better model. In this repo, I will experiment with an HMM ancestor
reconstruction algorithm. It will be based on the algorithm written by Bykova
et al (see citation below). There algorithm assumes a certain tree but
uncertain leafs states. I will need to impose a single-birth restriction on
this algorithm. I have a rough approach mapped out for handling this
restriction, but it is hard to express in text. 

Since the trees my dear users will give me are full of polytomies and have no
branch lengths, I will build a binary tree for each phylostratigraphic level
from the score data. This is a little tricky since the only data I have to work
with is sequence similarity of the query species to each target species. I can
build a tree from this, though. I can build a correlation matrix between scores
across all target species in a phylostratum, then use this as a distance matrix
in a standard tree building algorithm (e.g. neighbor-joining or UPGMA). This
may be a rather unwholesome thing to do, but we work with what we are given,
right?


# References

@article{bykova2013hidden,
  title={Hidden Markov models for evolution and comparative genomics analysis},
  author={Bykova, Nadezda A and Favorov, Alexander V and Mironov, Andrey A},
  journal={PloS one},
  volume={8},
  number={6},
  pages={e65012},
  year={2013},
  publisher={Public Library of Science}
}
