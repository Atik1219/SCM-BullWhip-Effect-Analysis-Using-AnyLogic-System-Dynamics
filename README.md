Demand variability typically originates at the customer level but becomes increasingly amplified as orders propagate upstream across a supply chain, resulting in the well-known bullwhip effect. This study develops a system dynamics simulation model of a four-echelon network comprising customer, retailer, distributor, and factory implemented in AnyLogic to investigate the operational drivers of this amplification and its impact on supply chain resilience. The model incorporates stochastic customer demand, exponential smoothing–based forecasting, inventory cover policies, proportional correction rules, and physical shipment constraints. Simulation results show that delays in information updating and inventory adjustment, combined with higher safety-stock targets and longer release times upstream, lead to significant variance growth in production and inventory at the factory level. The proposed model provides a dynamic testbed for diagnosing bullwhip mechanisms and evaluating alternative policies that improve responsiveness and resilience in multi-echelon supply chains.


Problem Description:
Supply chains with multiple echelons are highly vulnerable to the bullwhip effect, where small fluctuations in customer demand propagate upstream and become amplified as orders and inventories swing excessively. These oscillations lead to increased holding costs, production inefficiencies, lost sales, and reduced service levels. In real-world industries especially those characterized by long lead times, capacity constraints, and high safety stock strategies this dynamic instability becomes a critical operational challenge. The modeled system represents a typical four-echelon distribution network consisting of a factory, distributor, retailer, and end customer. Customer demand is stochastic and fluctuates daily around an average of 5 units, creating uncertainty that moves through the network. Each echelon uses delayed exponential demand forecasting, safety-stock coverage policies, and proportional inventory adjustment rules. However, due to increasing lead times, larger target cover stocks, and slower response speeds toward the upstream end of the network, the factory experiences the most pronounced variability in both production and inventory levels. This model captures how decision-making delays, shipment release limits, and demand noise interact to intensify order variability and destabilize material planning. Stock balance, forecast updating, and replenishment equations enable realistic simulation of operational dynamics including capacity constraints, service failure, and backlog risks. By implementing performance measures—such as bullwhip ratios, service level, and resilience metrics (e.g., time-to-recover)—the model quantitatively evaluates how demand disturbances impact the flow of goods and information. Ultimately, this system dynamics formulation enables rigorous assessment of bullwhip drivers and provides a platform for testing improvement strategies that enhance responsiveness, efficiency, and supply chain resilience.

RQ: How do forecasting delays, inventory control policies, and replenishment flow constraints interact to amplify demand variability and influence resilience in a multi-echelon supply chain?

The primary contribution of this work is a modular system dynamics simulation model that quantifies bullwhip amplification and resilience performance across interconnected supply chain stages. Unlike traditional analytical studies, this model incorporates operational constraints (capacity limits, stock cover rules, handling delays) that realistically shape disruption response. The framework provides diagnostic performance insights and a controlled experimentation environment for strategically redesigning policies to mitigate the bullwhip effect and improve recovery behavior.

 

The stochastic mathematical model:
The replenishment policy used in this study follows the standard order-up-to formulation seen in stochastic lead-time demand inventory models, where replenishment equals forecasted demand plus a proportional safety correction based on inventory gap.

Modelling Approach: 

Retailer Demand:
ReDemand= max( 0, Exp_R + (DesInv_R - Retailer)/adj_R )
DesInv_R = Exp_R * coverDays_R
Exp_R = smooth(Cdemand, info_R)

Customer Level
End-user demand & service limitation

Parameter	Symbol	Value	Units	Purpose
Base daily demand	Cdemand0	60	units/day	Average customer demand
Demand noise amplitude	dNoise	10	units	Random variation bound
Retailer service capacity	retailerCap	20	units/day	Max customers served per day

Retailer Level
Forecasting + Safety stock + Replenishment control

Parameter	Symbol	Value	Units	Purpose
Cover duration	coverDays_R	20	days	Retailer safety stock policy    (1200 )
Smoothing time	info_R	1.5	days	Demand forecast delay
Adjustment time	adj_R	1	days	Speed of inventory correction
Shipment handling time	shipTimeR	1	day	Retailer → Customer release rate

Distributor Level
Greater uncertainty → larger cover stock
Parameter	Symbol	Value	Units	Purpose
Cover duration	coverDays_D	30	days	Distributor safety buffer   ((1800))
Smoothing time	info_D	2.0	days	Demand filtering delay
Adjustment time	adj_D	5.5	days	Slow correction reduces oscillation
Shipment handling time	shipTimeD	1.0	day	DC → Retailer shipping rate

Factory Level
Largest buffer + slowest reaction → highest Bullwhip sensitivity
Parameter	Symbol	Value	Units	Purpose
Cover duration	coverDays_F	60	days	Factory safety stock target (3600)
Smoothing time	info_F	1.7	days	Slow reaction → amplification
Adjustment time	adj_F	6	days	Long correction delay
Shipment handling time	shipTimeF	1.0	day	Production release to DC
Workforce size	staff	15	workers	Production capacity driver
Productivity rate	prodPerStaff	40	units/worker/day	Total ProdCap = staff × prodPerStaff

 



Feature	Research Benefit
Increasing uncertainty upstream	Shows strong bullwhip growth
Smoothing delays	Causes lag → oscillation → instability
Safety stock multipliers	Demonstrates resilience trade-off
Handling times	Prevents unrealistic negative inventory
Capacity constraint	Adds operational realism

The inventory response shows that small variations in customer demand are amplified at the retailer level, further magnified at the distributor level, and most intensified at the factory level due to longer smoothing, higher safety stock, and slower adjustment policies. The factory responds late to demand changes, resulting in significant oscillations in its inventory before stabilizing. This represents a realistic bullwhip amplification pattern and proves the dynamic validity of the model.

Demand & policies.
D(t) : customer demand rate
\widetilde{D_R}\left(t\right),\widetilde{D_D}\left(t\right),\widetilde{D_F}\left(t\right) = exponentially smoothed (expected) demand at retailer, distributor, factory.
c_R,c_D,c_F = cover (days of demand kept as inventory)
\tau_R,\tau_D,\tau_F = inventory adjustment times
\alpha_R,\alpha_D,\alpha_F  = information smoothing time constants
C_R = retailer service capacity (units/day), K_F  factory production capacity (units/day).

Stock: I_R\left(t\right),\ I_D\left(t\right),\ I_F\left(t\right) = retailer, distributor, factory inventory.

  
 
 

Figure: Stock presentation for 4 domains (Small change becomes big waves upstream
→ Bullwhip Verified)


Shipments (flows).
S_{RC}\left(t\right) = retailer→customer
S_{DR}\left(t\right) = distributor→retailer
S_{FD}\left(t\right) = factory→distributor
P\left(t\right) = factory production inflow to I_F
h_R,h_D,h_F  = minimum time to release one stock’s content (days)

Desired Inventory levels:
I_R^\ast\left(t\right)=c_R\thinsp\widetilde{D_R}\left(t\right),
I_D^\ast\left(t\right)=c_D\thinsp\widetilde{D_D}\left(t\right),
I_F^\ast\left(t\right)=c_F\thinsp\widetilde{D_F}\left(t\right).

Order/production decision rules:
R\left(t\right)=\max{!}\left\{0,\ \widetilde{D_R}\left(t\right)+\frac{I_R^\ast\left(t\right)-I_R\left(t\right)}{\tau_R}\right\},
Q\left(t\right)=\max{!}\left\{0,\ \widetilde{D_D}\left(t\right)+\frac{I_D^\ast\left(t\right)-I_D\left(t\right)}{\tau_D}\right\},
P^{\mathrm{req}}\left(t\right)=\max{!}\left\{0,\ \widetilde{D_F}\left(t\right)+\frac{I_F^\ast\left(t\right)-I_F\left(t\right)}{\tau_F}\right\},
\emsp\emsp
P\left(t\right)=\min{\{}K_F,\ P^{\mathrm{req}}\left(t\right)\}.

	Sterman, J. D. (1989). Modeling managerial behavior: Misperceptions of feedback in a dynamic decision making experiment. Management science, 35(3), 321-339.

Physical shipment constraints (release-rate)
S_{RC}\left(t\right)=min\{\thinsp I_R/h_R,\ C_R,\ D\left(t\right)
S_{DR}\left(t\right)=min\{\thinsp I_D/h_D,\ R\left(t\right)
S_{FD}\left(t\right)=min\{\thinsp I_F/h_F,\ Q\left(t\right)


	Shipments are constrained by the minimum between desired order and the stock release capability, preventing the system from shipping more than physically available, a standard mechanism in industrial dynamics and bullwhip modeling (Forrester, 1961; Sterman, 2000; Disney & Towill, 2003).

Stock balance (system dynamics):
\dot{I_R}\left(t\right)=S_{DR}\left(t\right)-S_{RC}\left(t\right),
\dot{I_D}\left(t\right)=S_{FD}\left(t\right)-S_{DR}\left(t\right),
\dot{I_F}\left(t\right)=P\left(t\right)-S_{FD}\left(t\right).

Echelon	What happens	Why
Retailer	Slight inventory drop → orders +3	reacts fast
Distributor	Sees retailer fluctuation later → orders +5	higher safety stock and delay
Factory	Discovers surge too late → produces +10	slow forecast + capacity limit

Each stage tries to maintain its desired stock and meet forecast demand. Because upstream stages react slower, hold more inventory, and face capacity limits, they correct their errors too strongly and too late. This naturally transforms small downstream demand variations into large upstream fluctuations — the bullwhip effect.

Layer	Flow Direction	What Happens Here	Variables in Model
Customer	Demand → Retailer	Random demand disturbances enter system	Cdemand
Retailer	Ship → Customer; Order → Distributor	Immediate demand response, small forecast delay	Retailer, ReDemand, flow2
Distributor	Ship → Retailer; Order → Factory	Larger safety stock, slower reaction	Distributor, DDmnd, flow1
Factory	Produce → Distributor	Slowest adaptation + capacity limit	PSTK, ProdRate, flow



Demand Signal	Retailer	Distributor	Factory
Reacts	Fast	Slower	Slowest
Inventory target	Medium	Higher	Highest
Response strength	Mild	Medium	Strong
Capacity limits	No	Low	High
Result	Small variation	Bigger swings	Max oscillation

 


VMI VCON:
