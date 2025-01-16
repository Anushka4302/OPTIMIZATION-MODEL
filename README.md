# OPTIMIZATION-MODEL.
import pulp

def solve_production_problem():
    """
    Solve a linear programming problem for a production optimization scenario.
    """
    # Define the problem
    prob = pulp.LpProblem("Production_Optimization", pulp.LpMaximize)

    # Decision variables
    x1 = pulp.LpVariable("Product_A", lowBound=0, cat='Continuous')  # Units of Product A
    x2 = pulp.LpVariable("Product_B", lowBound=0, cat='Continuous')  # Units of Product B

    # Objective function: Maximize profit
    prob += 20 * x1 + 30 * x2, "Total Profit"

    # Constraints
    prob += 2 * x1 + 4 * x2 <= 100, "Labor Hours"
    prob += 3 * x1 + 1 * x2 <= 90, "Material Units"
    prob += x1 <= 30, "Demand for Product A"
    prob += x2 <= 20, "Demand for Product B"

    # Solve the problem
    prob.solve()

    # Display the results
    print("Status:", pulp.LpStatus[prob.status])
    print("Optimal Production Plan:")
    print("Product A (units):", pulp.value(x1))
    print("Product B (units):", pulp.value(x2))
    print("Total Profit:", pulp.value(prob.objective))

    return {
        "status": pulp.LpStatus[prob.status],
        "Product_A_units": pulp.value(x1),
        "Product_B_units": pulp.value(x2),
        "Total_Profit": pulp.value(prob.objective)
    }

if __name__ == "__main__":
    # Solve the production optimization problem
    result = solve_production_problem()

    # Insights
    print("\nInsights:")
    if result["status"] == "Optimal":
        print("The optimal production plan meets all constraints and maximizes profit.")
    else:
        print("The problem could not be solved optimally. Check constraints and inputs.")
