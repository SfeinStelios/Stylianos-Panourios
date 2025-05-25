# Synthetic Societies Simulator (Prototype)
# Purpose: Model ideological shifts and their impact on psychological values and economic behavior

# --- Agent Definition ---
class Agent:
    def __init__(self, id, region):
        self.id = id
        self.region = region
        self.traits = self.initialize_traits(region)

    def initialize_traits(self, region):
        # Starting values based on continental templates
        templates = {
            'China': {
                'collectivism': 0.85,
                'patriotism_ideological': 0.90,
                'patriotism_behavioral': 0.50,
                'trust_in_institutions': 0.80
            },
            'USA': {
                'collectivism': 0.30,
                'patriotism_ideological': 0.70,
                'patriotism_behavioral': 0.40,
                'trust_in_institutions': 0.45
            },
            'Europe': {
                'collectivism': 0.60,
                'patriotism_ideological': 0.55,
                'patriotism_behavioral': 0.30,
                'trust_in_institutions': 0.50
            }
        }
        return templates.get(region, {})

# --- Behavioral Module Definitions ---
class TheoryModule:
    def __init__(self, name, activation_threshold):
        self.name = name
        self.activation_threshold = activation_threshold

    def should_activate(self, trait_value):
        return trait_value >= self.activation_threshold

    def apply(self, agent):
        raise NotImplementedError

class ProspectTheory(TheoryModule):
    def apply(self, agent):
        # Simulate increased saving when risk aversion is high
        if self.should_activate(agent.traits.get('trust_in_institutions', 0)):
            return 'Increase Saving Behavior'
        return 'No Change'

class SocialIdentityTheory(TheoryModule):
    def apply(self, agent):
        if self.should_activate(agent.traits.get('collectivism', 0)):
            return 'Align with Group Norms'
        return 'No Change'

class NarrativeEconomics(TheoryModule):
    def apply(self, agent):
        if self.should_activate(agent.traits.get('patriotism_ideological', 0)):
            return 'Support Domestic Campaigns'
        return 'No Change'

# --- Simulation Environment ---
def simulate(agent, theory_modules):
    results = {}
    for module in theory_modules:
        outcome = module.apply(agent)
        results[module.name] = outcome
    return results

# --- Example Simulation ---
if __name__ == "__main__":
    agent_usa = Agent(id=1, region='USA')

    theories = [
        ProspectTheory(name='Prospect Theory', activation_threshold=0.5),
        SocialIdentityTheory(name='Social Identity Theory', activation_threshold=0.6),
        NarrativeEconomics(name='Narrative Economics', activation_threshold=0.75)
    ]

    outcomes = simulate(agent_usa, theories)
    for theory, result in outcomes.items():
        print(f"{theory}: {result}")
