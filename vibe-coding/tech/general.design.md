## General Patterns

- First Principles: Identify core requirements and system boundaries  
- YAGNI (You Aren't Gonna Need It): Implement only what is currently necessary  
- KISS (Keep It Simple, Stupid): Maintain simplicity in design and implementation  
- SOLID: Follow Single Responsibility, Open-Closed, and other principles for object-oriented/modular design  
- DRY (Don't Repeat Yourself): Eliminate duplication by extracting common logic  

### Dynamically Adjust Priority Based on Context

- Architecture/Requirements Analysis (Project Kickoff): First Principles → YAGNI → KISS → SOLID → DRY
- Feature Iteration/Incremental Development: YAGNI → KISS → SOLID → DRY → First Principles
- Utility Functions/Library Implementation: KISS → DRY → YAGNI → SOLID → First Principles
- Complex Business Components/Object-Oriented Modeling: First Principles → SOLID → YAGNI → KISS → DRY

### Implementation Details

- Make sure the low-level implementation is *testable* and *reusable*.
- Declare good `interfaces` and `APIs` for the high-level implementation.