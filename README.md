# DraconiaRP

State server in cpp.
Api can read full state: (maybe returned as json or specially crafted format) or context relevant state. Like api.GetContext("Clothes", vector<Characters>) returns the clothes the characters requested are wearing.
Api can change state: api.SetState("Player", "Character1", "Clothes", "left Leg", One of the available objects in a list) see addObject
  api.GetPlayerNames()
  api.GetLocationNames()
  api.AddPlayer()
only added players and locations can be used to prevent contradictions. 
If requested thing does not exist, give a precise error message and automatically re prompt. 
api.addObject(Object object)
- object has a type: (Clothing) and can have n=10 attributes with word limit of 10. Like: Object: "Knee High Socks" | Attrib1: "cotton", Attrib2: "pink and white stripes"

RegisterItemTemplate(...) defines the blueprint (id: "knee_high_socks_pink", type: Clothing, static attributes).

SpawnItem(template_id, owner_or_location) creates a unique instance (instance_id: "item_42") that can hold dynamic state (wet, torn, equipped_slot: LeftLeg).

Maybe every Objects always has: Owner: None, Location: None, State vector: Dry, dirt on lower half, torn on upper thight, CreationDate: 12948, TimeAlive: 124598 

The the frontend should not display every message, the LLM when recieving the prompt can communicate with the stateserver as much as it likes (or max number of calls) to produce a precise answer. Frontend only displays finished answer. 

Only use GetContext() for details, always supply  important baseline.

EquipItem(entity_id, slot, item_instance_id) (automatically unequips from previous location/slot)

each turn's mutations as a SQLite transaction. should be rollbackable. 

World simulation, clock. Every final prompt should advance time by n or 0. api.advanceTime(5minutes).
