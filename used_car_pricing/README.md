# Methodology

The used car dataset was explored and cleaned using industry-standard techniques. Data gaps and quality problems were addressed. To deal with wide variation in how models were reported, a model name standardization process was applied. Variables were examined to identify which are important in determining sale prices.

See notebook [here](https://github.com/jshuma/experimental/blob/main/used_car_pricing/prompt_II.ipynb).

# Findings

Model, year, and condition are the most important factors driving the value of a car, so dealers are advised to prioritize these factors in selecting inventory.

American "muscle cars" and trucks were particularly notable in the top selections. The overall top models were:
- Corvette (all types)
- Ford Mustang
- Jeep Wrangler
- Chevrolet 3500
- Chevrolet Camaro
- Chevrolet 2500
- Toyota Tundra
- Ford F-350
- Toyota Tacoma
- Ford F-250

In contrast, compact cars and Japanese cars were decidedly unpopular with customers, and had a negative impact on price, with the following cars being lowest priced:
- Chevrolet Cruze
- Hyundai Elantra
- Nisaan Sentra
- Kia Soul
- Volkswagen Jetta
- Hyandai Sonata
- Toyota Prius
- Toyota Corolla
- Nissan Altima
- Ford Fusion

Fuel type (particularly hybrid fuel) was not a help in pricing, and odometer readings did not significantly impact pricing (at least, not in a way that outweighed the model year that more closely correlates with price).

State of registration was also significantly correlated with price, but it is unclear whether this amounts to a significant enough effect to justify transporting inventory.

# Recommendations

Higher-valued cars are likely to bring higher margins, and these are likely to perform better in outweighing transaction costs and other overhead. Therefore, clients are advised to focus on highest-valued variables, in particular:
- Model: Focus on those with higher sale price, particularly American muscle cars and trucks. Avoid compact cars and Japanese cars.
- Prefer recent model years.
- Prefer cars in good condition.
- Disregard odometer readings.
