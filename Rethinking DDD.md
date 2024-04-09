# Rethinking Domain-Driven Design: From Nouns to Verbs

## Introduction
Domain-Driven Design (DDD) has long been a staple tool in modeling complex business logic, offering a structured approach to dealing with the complexities of business requirements. Traditionally, DDD has employed a noun-based approach, focusing on defining aggregates as key structures which own the behaviors (the 'verbs'), and Bounded Contexts as service or subdomain boundaries. This blog post explores the problems this approach creates and proposes transitioning to a more adaptive verb-based modeling approach.

## The Evolving Nature of Business Use Cases
Evolution is an intrinsic nature of business use cases. This is driven by two factors.

First, it is inherent in the nature of business itself: as a business develops and adapts to new market trends, competitive pressures, and customer needs, so too do its technical and operational requirements evolve.

Second, these use cases are frequently initiated with limited understanding from both the business and technical teams. This initial gap in understanding means that as the use cases are iterated upon by both teams, more knowledge is acquired, more insight gained, making adjustments and refinements inevitable.

Naturally evolving business use cases call for an easily evolving business logic modeling approach.

## Traditional DDD: A Constraint on Evolution
In traditional DDD, the model is built on the assumption of non-overlapping aggregates and bounded contexts. Both aggregates and bounded contexts, while initially offering clarity, can block the application as a whole from effective evolution. When new requirements necessitate the inclusion of more states that are already captured by other aggregates or bounded contexts, traditional DDD discourages direct state sharing. The solution offered instead is often to raise DomainEvents to be processed by the relevant aggregates/bounded contexts. This event processing is often asynchronous, and many times intra-process or intra-machine. This approach, while adhering to the boundaries that were established in the early phase of the project, inadvertently creates an asynchronous, disjointed software process for what is essentially a synchronous and cohesive business workflow. The solution offered by traditional DDD can be a significant source of the complexity that DDD started out to reduce initially.

Furthermore, in traditional DDD, aggregates are usually the container multiple behaviors (business cases) and the aggregation of their states. As these business cases evolve, often independently, the aggregated states they share need to evolve accordingly. This can lead to either growing the aggregates unwieldy, or to forcibly externalize the states with events. This is a choice between bloated aggregates or bloated domain events. Neither of which is ideal. And both are the source of complexity rather than the solution of it.

## Shifting to a Verb-Based Approach
Recognizing these problems, a shift towards a verb-based approach in DDD becomes critical. This approach views the business cases as independent entities that fully own their operational contexts (states). This allows the states to evolve freely together with the business use cases, free from the artificial constraints of traditional DDD.

The distinction between the noun-based and verb-based approaches comes down to the simple difference shown below.

Traditional noun-based DDD might look like:
```csharp
class MyAggregateRoot {
    AggregatedStates _myStates;
    void MyBusinessCase1();
    void MyBusinessCase2();
}
```

In the traditional approach:

The MyAggregateRoot class encapsulates the state (AggregatedStates _myStates) for both use cases (MyBusinessCase1, MyBusinessCase2). This pattern focuses on modeling the aggregate (noun) and the use cases are part of the noun.

In contrast, a verb-based pattern would be represented as:
```csharp
static MyStatefulWorld1 MyBusinessCase1(MyStatefulWorld1 states);
static MyStatefulWorld2 MyBusinessCase2(MyStatefulWorld2 states);
```
In the verb-based approach:

Each business case is represented as a function. The states (e.g., MyStatefulWorld1, MyStatefulWorld2) are the input and output types owned by the functions and evolve together with the function and independently of each other.

## Advantages of the Verb-Based Approach
The verb-based approach in DDD presents several advantages, particularly in its flexibility and responsiveness to the evolving nature of business requirements.

Firstly, it allows business cases to evolve independently, treating each as a unique function with its own stateful world. This independence means that changes or developments in one area of the business do not necessitate forced adjustments in others, fostering a more agile and adaptive environment.

Moreover, this approach permits the stateful world to evolve more freely. Unlike traditional DDD, where rigid aggregate boundaries can restrict evolution, a verb-based system encourages a more organic growth or shrinking of the domain model. This growth reflects the actual needs and dynamics of the business, not the predetermined structures.

Verb-based systems still offer robust validation, similar to what aggregates provide. For instance, MyStatefulWorld1 would encompass all necessary validation rules to ensure its integrity, just as an aggregate was proposed to do. This ensures that while the system is flexible, it does not compromise on the consistency and reliability of the data.

Additionally, verb-based systems allow for the sharing of stateful worlds between functions. If the stateful worlds of two business cases converge, they can be defined as the same type and shared. This sharing is based on practicality and actuality, rather than adhering to a strict design guideline. It's a more pragmatic approach that aligns with how business cases naturally evolve and intersect.

The concept of bounded contexts remains relevant and applicable in a verb-based system. However, these contexts are defined based on actual needs – such as real divisions within business or technical teams – and can evolve as requirements change. This flexibility ensures that bounded contexts are not just theoretical constructs but practical tools that adapt to the shifting realities of the business environment.

## Conclusion
In summary, a verb-based system in DDD brings a dynamic and realistic approach to handling business cases. It combines the advantages of flexibility, independent evolution, and practical bounded contexts, while maintaining the necessary validation and consistency checks, making it a compelling alternative to traditional noun-based DDD in modeling complex and dynamic business scenarios.