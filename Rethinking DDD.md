# Evolving Perspectives in Domain-Driven Design: From Nouns to Verbs

## Introduction
Domain-Driven Design (DDD) has long been a staple tool box in modeling complex business logic, offering a structured approach to dealing with the complexities of business requirements. Traditionally, DDD has employed a noun-based approach, focusing on defining aggregates as key structures which owns the behaviors (the 'verbs'). This blog post explores problems this approache creates and proposes transitioning to a more adaptive verb-based modeling approach. 

## The Evolving Nature of Business Use Cases
Business use cases often start off poorly understood and inevitably evolve over time. This evolution is driven by two factors. 

First, it is inherent in the nature of business itself: as a business develops and adapts to new market trends, competitive pressures, and customer needs, so too do its technical and operational requirements evolve.

Second, these use cases are frequently initiated with limited understanding by both the business and technical teams. This initial gap in understanding means that as the use cases are iterated upon by both teams, more knowledge is acquired, making adjustments and refinements inevitable. 

### Traditional DDD: A Constraint on Evolution
In traditional Domain-Driven Design (DDD), the model is built on the assumption of non-overlapping aggregates and bounded contexts. This design choice, while initially offering clarity, can lead to issues as business requirements evolve. When new requirements necessitate the inclusion of states that are already captured by other aggregates or bounded contexts, traditional DDD discourages direct state sharing. Instead, it recommends raising domain events to be processed asynchronously by the relevant aggregates/bounded contexts. This approach, while ensuring separation of concerns, can inadvertently create an asynchronous software process for what is essentially a synchronous business process, overlooking the significant complexity the asynchronous software process adds.

Furthermore, traditional DDD mandates that aggregate to be the container of both states and behaviors(business cases). As these business cases evolve, often independently, the associated states they share need to evolve accordingly. This can lead to either growing the aggregates unwieldy, or to forcibly externalize the states. This is a choice between bloated aggregates or bloated asynchronous domain events. Neither of which is ideal. The end result is often that DDD becomes the source of complexity rather than the colution to complexity.

## Shifting to a Verb-Based Approach
Recognizing these problems, a shift towards a verb-based approach in DDD becomes essential. This approach views the business cases as an independent entities that fully owns their operational context(states). This allows the states to be evolved freely together with the business use cases, free from the artificial constraints from the traditional DDD.

The distinction between these the noun-based and verb-based approaches come down to the simple difference shown below.

Traditional noun-based DDD might look like:
```csharp
class MyAggregateRoot {
    MyStates _myStates;
    void MyBusinessCase1();
    void MyBusinessCase2();
}
```
In this traditional approach:

The `MyAggregateRoot` class encapsulates the state (`MyStates _myStates`) and owns the behaviors `(MyBusinessCase1, MyBusinessCase2)`. Business cases are methods within the aggregate, operating on a shared state. This pattern encourages modeling around entities (nouns) and their lifecycle, with business cases tied to these entities.

In contrast, a verb-based pattern would be represented as:
```csharp
static MyStatefulWorld1 MyBusinessCase1(MyStatefulWorld1 states);
static MyStatefulWorld2 MyBusinessCase2(MyStatefulWorld2 states);
```
In this verb-based approach:

Each business case is represented as a function. The states (e.g. MyStatefulWorld1, MyStatefulWorld2) are the input and output type owned by the functions. And the functions are independent to each other.

There's a clear separation between the the functions the states, as oppose to them being encapsulated together inside aggregates..

## Advantages of the Verb-Based Approach
A verb-based approach in Domain-Driven Design (DDD) presents several advantages, particularly in its flexibility and responsiveness to the evolving nature of the business requirements. 

Firstly, it allows business cases to evolve independently, treating each as a unique function with its own stateful world. This independence means that changes or developments in one area of the business do not necessitate forced adjustments in others, fostering a more agile and adaptive environment.

Moreover, this approach permits the stateful world to evolve more freely. Unlike traditional DDD, where rigid aggregate boundaries can restrict evolution, a verb-based system encourages a more organic growth of the domain model. This growth reflects the actual needs and dynamics of the business, not just predetermined structures.

Verb-based systems still offer robust validation, similar to what aggregates provide. For instance, MyStatefulWorld1 would encompass all necessary validation rules to ensure its integrity, just as an aggregate would. This ensures that while the system is flexible, it does not compromise on the consistency and reliability of the data.

Additionally, verb-based systems allow for the sharing of stateful worlds between functions. If the stateful worlds of two business cases converge, they can be defined as the same type and shared. This sharing is based on practicality and actuality, rather than adhering to a strict design guideline. It's a more pragmatic approach that aligns with how business cases naturally evolve and intersect.

The concept of bounded contexts remains relevant and applicable in a verb-based system. However, these contexts are defined based on actual needs – such as real divisions within business or technical teams – and can evolve as requirements change. This flexibility ensures that bounded contexts are not just theoretical constructs but practical tools that adapt to the shifting realities of the business environment.

## Conclusion
In summary, a verb-based system in DDD brings a dynamic and realistic approach to handling business cases. It combines the advantages of flexibility, independent evolution, and practical bounded contexts, while maintaining the necessary validation and consistency checks, making it a compelling alternative to traditional noun-based DDD in modeling complex and dynamic business scenarios.