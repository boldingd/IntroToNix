# Pipelines

Pipelines are a powerful feature of \*Nix systems.
We have said before that many \*Nix utilities are designed to perform one single task very well; this is true, but is only half of the story.
Many \*Nix utilities are also designed to be connected together in sequence, so that the *output* of one command can be used as the *input* to the *next* command in the sequence.
We call such a sequence a *pipeline*, and they can allow \*Nix users to perform, with just the command line and common Unix tools, the kinds of data-manipulation that Windows users (for example) might have to write a special program to perform.
