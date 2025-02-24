# .osrm.cnbg_to_ebg
Maps node based graph to edge based graph.     

## List

```bash
tar -tvf nevada-latest.osrm.cnbg_to_ebg
-rw-rw-r-- 0/0               8 1970-01-01 00:00 osrm_fingerprint.meta
-rw-rw-r-- 0/0               8 1970-01-01 00:00 /common/cnbg_to_ebg.meta
-rw-rw-r-- 0/0         6085136 1970-01-01 00:00 /common/cnbg_to_ebg
```

## osrm_fingerprint.meta
- [osrm_fingerprint.meta](./fingerprint.md)

## /common/cnbg_to_ebg, /common/cnbg_to_ebg.meta
Stores [NBGToEBG](https://github.com/Telenav/osrm-backend/blob/0b461183b97de493983ba44749c772719849fd3e/include/extractor/nbg_to_ebg.hpp#L13) mapping.     
Refer to [Understanding OSRM Graph Representation - Basic Changes of Convert OSM to OSRM Edge-expanded Graph](https://github.com/Telenav/open-source-spec/blob/master/osrm/doc/understanding_osrm_graph_representation.md#basic-changes-of-convert-osm-to-osrm-edge-expanded-graph) to understand what is the edge-expanded graph and how does it be generated.         

### Layout
![](./graph/map.osrm.cnbg_to_ebg.common.cnbg_to_ebg.png)

### Implementation
The [NBGToEBG](https://github.com/Telenav/osrm-backend/blob/0b461183b97de493983ba44749c772719849fd3e/include/extractor/nbg_to_ebg.hpp#L13) mapping is a vector which will be generated by [GenerateEdgeExpandedNodes](https://github.com/Telenav/osrm-backend/blob/0b461183b97de493983ba44749c772719849fd3e/src/extractor/edge_based_graph_factory.cpp#L262). Then use [files::writeNBGMapping](https://github.com/Telenav/osrm-backend/blob/0b461183b97de493983ba44749c772719849fd3e/src/extractor/edge_based_graph_factory.cpp#L263) to write it to `.osrm.cnbg_to_ebg` file directly.     

```c++
    TIMER_START(generate_nodes);
    {
        // [Jay] generate the NBGToEBG mapping and write to file
        auto mapping = GenerateEdgeExpandedNodes(way_restriction_map);
        files::writeNBGMapping(cnbg_ebg_mapping_path, mapping);
    }
```
Each [NBGToEBG](https://github.com/Telenav/osrm-backend/blob/0b461183b97de493983ba44749c772719849fd3e/include/extractor/nbg_to_ebg.hpp#L13) structure see below definition:       
```c++
// Mapping between the node based graph u,v nodes and the edge based graph head,tail edge ids.
// Required in the osrm-partition tool to translate from a nbg partition to a ebg partition.
struct NBGToEBG
{
    NodeID u, v;
    NodeID forward_ebg_node, backward_ebg_node;
};
```
