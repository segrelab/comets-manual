This protocol demonstrates the capacity of COMETS to simulate reactions
occuring in the extracellular environment.

Extracellular reaction information is included in the COMETS layout file, in
blocks with the following format:

REACTANTS
        rxnIdx metIdx order k   //elemental rxn
        rxnIdx metIdx km        //enzyme catalyzed
    ENZYMES
        rxnIdx metIdx kcat
    PRODUCTS
        rxnIdx metIdx stoich
    //

Each reactant or product in a given reaction should appear on its own line,
using the same integer rxnIdx per extracellular reaction. The metIdx field
corresponds to a metabolite’s position in the world_media block. The system
determines whether a reaction is elemental or enzymatic based on the presence
of an entry in the ENZYMES block. In the case of elemental reactions, only
the first entry for the rate constant k is used.

#TODO: example simulation, implement in py toolbox
