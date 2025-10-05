# Linear Optimization of Humanitarian Aid Logistics
Python-based linear programming model to optimize humanitarian logistics, balancing delivery constraints and improving operational efficiency for disaster relief.

## Introduction
The 2004 Indian Ocean earthquake and subsequent tsunami devastated the surrounding countries, with the most severe impact on the west coast of Sumatra (Wiharta et al. 2008, p. 87). As a response, other nations provided swift aid by sending supplies and donations to affected areas. While there was an outpouring of aid, distributing said aid in a timely and efficient manner was another challenge.

In order to explore the problem of aid distribution, we focused on the region of Aceh in Indonesia, one of the main areas directly impacted by the natural disasters, providing a specific location and conditions. Aid distribution in this context refers to the movement of supplies from designated supply nodes (several airports that hold the supplies initially) to demand nodes (city centres which could further distribute aid locally), while fulfilling supply requirements from the demand nodes. Furthermore, since this distribution should be timely, we aim to minimize the distance that the aid travels, in turn minimizing the time travelled.

We referenced the following map from the two-year tsunami progress report by the International Federation of Red Cross and Red Crescent Societies, which highlights demand locations. We used the centres of each province as demand nodes in our analysis.

We expanded upon the transportation problem by considering multiple supply types, as well as limiting the transport capacity from each supply location. The range of supplies included medical items and construction materials for shelters, and every central town required a different amount of each type of aid depending on the severity of the natural disaster in that area. Furthermore, we decided that for our model of this problem, the method of transportation would be helicopters, getting rid of the need to map distance through roads and instead using the Euclidean distance derived from geographic coordinates.

**Variables:**
- `m`: total number of supply nodes, `i` in {1, …, m}
- `n`: total number of demand nodes, `j` in {1, …, n}
- `r`: total number of aid types, `k` in {1, …, r}
- `x[i,j,k]`: amount (kg) of aid type `k` delivered from supply node `i` to demand node `j`
- `c[i,j]`: cost of delivering 1 kg of any aid type from supply node `i` to demand node `j` (e.g., Euclidean distance)
- `s[i,k]`: kilograms of aid type `k` available at supply node `i`
- `d[j,k]`: kilograms of aid type `k` required by demand node `j`
- `t[i]`: transport capacity for supply node `i` (kg·km), e.g., fuel available for helicopters

**Objective Function:**
```
Minimize: sum(i=1 to m) sum(j=1 to n) sum(k=1 to r) c[i,j] * x[i,j,k]
```

**Constraints:**

1. **Supply constraints** (for every `(i,k)`):
```
sum(j=1 to n) x[i,j,k] <= s[i,k]
```

2. **Demand constraints** (for every `(j,k)`):
```
sum(i=1 to m) x[i,j,k] >= d[j,k]
```

3. **Transport capacity constraints** (for every `i`):
```
sum(j=1 to n) sum(k=1 to r) c[i,j] * x[i,j,k] <= t[i]
```

4. **Non-negativity constraints** (for every `(i,j,k)`):
```
x[i,j,k] >= 0
```

The optimization problem above models the movement of aid relief from main supply locations to affected areas during a humanitarian catastrophe. We aim to minimize the distance the transporters travel to deliver different aid types to the impacted locations to efficiently distribute the available aid appropriately, represented by the objective function in the transport problem. The constraints map to real-world assumptions $-$ namely that each supply location has limited aid resources available to distribute in the area and each affected area has a minimum quota of aid relief necessary for its stability. They also posit that the transport capacity of each supply location is limited and that the amount of aid must be non-negative values. We account for a limited number of transport vehicles available and calculate the cost of transporting a certain amount of aid to a location.

In our model, we assume that the time it takes to deliver supplies is proportional to the distance travelled. Naturally, we restrict the amount of helicopters at a supply node to a discrete value (i.e. one whole helicopter must travel from a supply node to a demand node), however supply quantities may be continuous (i.e. measured by weight). We also assume there is at least as much supply as demand, and that each route is a return trip between each supply and demand node

## Results and Further Remarks

From our exploration, we found that a further extension could be to explore different modes of transport (sea, land, air) and how that would impact efficiency and resource allocation. Each mode introduces unique considerations such as speed, capacity, cost efficiency, and route constraints. One possible approach would be to introduce multi-modal transport optimization, where aid shipments could transition between different modes based on cost and time trade-offs. For instance, helicopters could be used for urgent deliveries to remote locations, while trucks or boats could handle bulk shipments for efficiency. This would require incorporating additional constraints and variables in our model to account for transfer points, mode-switching costs, and different travel speeds.

Another important extension would be to refine routing strategies. In our current model, helicopters deliver supplies directly from supply nodes to demand nodes. However, allowing helicopters to visit multiple demand nodes in a single trip before returning to a supply node could improve efficiency. This change would alter the problem structure, shifting it from a linear transport optimization to a combinatorial optimization routing problem. Optimizing routes for multi-stop deliveries while considering additional constraints such as fuel limitations, time or priority constraints, while minimizing total distance traveled. By optimizing both route selection and resource allocation, this approach could significantly enhance the effectiveness of humanitarian aid allocation efforts.

Realistically, many organizations – including humanitarian agencies, governments, and military forces from all around the world – collaborate during and after a disaster, each with different resources, transportation capacities and priorities. This is a challenge highlighted in the sources we’ve used for this research on the 2004 tsunami, where much of the aid either didn’t reach the areas that needed it most, was duplicated, or was not the type of aid required. It would be useful to consider how these organizations can coordinate their efforts to avoid duplication of resources, reduce wastage, and ensure efficient aid distribution.
