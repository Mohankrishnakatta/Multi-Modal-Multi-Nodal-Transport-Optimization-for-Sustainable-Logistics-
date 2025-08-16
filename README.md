# Multimodal Multinodal Transport Optimization

## Project Description
Optimizes freight movement between Indian cities using multiple transport modes — airways, railways, roadways, and waterways. The model minimizes either total cost, CO₂ emissions, or travel time under flow-balance constraints, and generates interactive route maps.

## Tools Used
1. Pyomo for mathematical optimization modeling.
2. Gurobi/CBC/HiGHS as MILP solvers (Gurobi used by default).
3. Folium for interactive route maps.
4. Geopy for geocoding city coordinates.

## Data
- Predefined distance matrices for 11 cities across four modes.
- Mode parameters:
  - Costs per km: air=₹100, rail=₹8, road=₹15, water=₹5.
  - Emissions per km (kg CO₂): air=1.58, rail=0.0095, road=0.18, water=0.0035.
  - Speeds (km/h): air=800, rail=70, road=50, water=20.

## Optimization Objectives Implemented
1. Cost minimization
2. Emission minimization
3. Time minimization
4. Optional: total-time constraint (user-specified hours)

## How to Run
1. Install requirements:
   - pip install pyomo folium geopy
   - Install a solver (Gurobi recommended; CBC/HiGHS also compatible via Pyomo).
2. Open and run Multinode_Multimodal.ipynb.
3. Provide inputs when prompted:
   - Source city, Destination city, Shipment weight, Max allowed time (hours).
4. Outputs:
   - Console route summaries for each objective.
   - HTML maps saved as route_map_cost.html, route_map_emission.html, route_map_time.html.

## Route Visualization
- Red markers: cities used in the model.
- Blue polylines: chosen legs with popups showing mode, distance, time, cost, and emissions.

## Example Results (Kochi → Guwahati)
| Objective | Route (legs) | Distance | Time | Cost | Emissions |
|-----------|---------------|----------|------|------|-----------|
| Cost | Kochi→Chennai (Rail), Chennai→Kolkata (Water), Kolkata→Guwahati (Rail) | 700 + 1400 + 600 km | 10.0h + 70.0h + 8.6h | ₹5,600 + ₹7,000 + ₹4,800 | 6.65 + 4.90 + 5.70 kg |
| Emission | Kochi→Kolkata (Water), Kolkata→Guwahati (Rail) | 2,900 + 600 km | 145.0h + 8.6h | ₹14,500 + ₹4,800 | 10.15 + 5.70 kg |
| Time | Kochi→Bengaluru (Rail), Bengaluru→Kolkata (Air), Kolkata→Guwahati (Rail) | 590 + 1,470 + 600 km | 8.4h + 1.8h + 8.6h | ₹4,720 + ₹147,000 + ₹4,800 | 5.60 + 2,322.60 + 5.70 kg |

Note: Results depend on objective; air is fastest but most costly and carbon-intensive, waterways/rail often reduce cost/emissions with higher travel times.

## Key Functions
- optimize_objective(source, destination, weight, max_time_hours, obj_type, use_time_constraint=False)
  - obj_type ∈ {‘cost’, ‘emission’, ‘time’}
- create_route_map(route, nodes, title)
  - Saves interactive HTML map for the computed route.

## Assumptions & Limits
- Single-commodity continuous flow; no capacity or transshipment penalties beyond mode costs.
- Fixed average mode speeds and per-km cost/emission factors.
- Distances are static approximations (representative, not live network data).

## Future Enhancements
- Multi-objective Pareto front (cost–time–CO₂ trade-offs).
- Mode- and arc-specific capacity or availability windows.
- Live data integration (timetables, congestion, weather).
- Inventory holding and transfer-time penalties at intermediate nodes.
