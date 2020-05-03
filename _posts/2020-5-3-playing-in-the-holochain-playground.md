---
layout: post
title: Playing in the holochain playground
tags: [tools, concepts]
author: Guillem Córdoba 
---

The [holochain playground](https://holochain-open-dev.github.io/holochain-playground) is a tool intended for learning and understanding how holochain internal mechanisms work. It can also be used to understand better the entry graph that results of determined actions we can take. This is a very brief guide to get you started on how to play in the playground.

The playground is structured in a way that **mimics as much as possible how holochain would behave** under different conditions. Underneath the hood, it **maintains a list of conductors with cells connected** with one another in the same holochain network. All the sections we see in the playground are only **views of different parts of the same network**, and all **actions we take in those will affect the network as a whole**, like they would in a real happ.

Firstly, the playground is divided into two sections: designer and technical mode. Both modes are different perspectives that are looking at the same DNA and nodes. 

### Designer mode

In the left side of this mode is a high-level graph of all entries existing in the DHT: 

![](/blog/images/playground/playground1.png)

Here we see each entry as a node in the graph, and links between nodes represent links in holochain. Each node is labeled using its entry type and a number to differentiate between entries with the same type. These are the different things we can do in this mode:

- Select a node to inspect the entry's contents and metadata in the bottom right panel
- Hide all `AgentId` entries that don't have any link to any other entry with the checkbox in the bottom left of the entry graph panel
- Create new entries (more on this later)
- Dynamically change the number of nodes and the redundancy factor (more on this later)

### Technical mode

In the right side of this mode is a technical view of all the agents or conductors that exist in our DHT:

![](/blog/images/playground/playground2.png)

Each node in the DHT graph represents a conductor that exists in our network. The images that show every node as a different device type are chosen randomly, only there for easier understanding. The conductors that are connected to each other are neighbors: they communicate regularly to each other 


## Changing the number of nodes and the redundancy factor

## Creating entries and links

## Connecting to real nodes

## Export and import 