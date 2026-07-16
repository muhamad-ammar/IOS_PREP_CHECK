# iOS Technical Interview Checklist


Every line below is a checkbox. Register format for each: `Concept → 1-line definition → sample question → your 2-sentence answer`. Nothing here is a placeholder — every item is a named, concrete concept to look up and note down.

---

## MONDAY — 1 hr — Setup + Core Swift

- [x] Value types vs reference types — struct/enum (value) vs class (reference); copy-on-write behavior
- [x] Optionals — `?`, `!`, `if let`, `guard let`, `??` (nil-coalescing), optional chaining
- [x] Closures — syntax, trailing closure syntax, capture lists `[weak self]`, escaping vs non-escaping closures
- [x] Encapsulation — access control keywords: `private`, `fileprivate`, `internal`, `public`, `open`
- [x] Inheritance — subclassing, `override`, `super`
- [x] Polymorphism — method overriding vs overloading
- [x] Protocol-Oriented Programming — protocols with default implementations via extensions; why Apple recommends POP over class inheritance
- [x] Register tabs created: Swift/OOP, Objective-C, UIKit vs SwiftUI, Performance/Profiling, Concurrency, Architecture, Networking/Persistence, Testing/CleanCode/CI-CD/Agile, DSA, Behavioral

**Sample questions to prep answers for:**
- [x] "What's the difference between a struct and a class?"
- [x] "What does `[weak self]` do and when do you need it?"
- [x] "What's the difference between `guard let` and `if let`?"

---

## TUESDAY — 5 hrs — Objective-C, UIKit, SwiftUI, Memory

### Block 1 (60 min) — Objective-C essentials
- [x] Basic syntax: `@interface` / `@implementation` / `@end`
- [x] Message passing: `[object method:param]` vs Swift's `object.method(param)`
- [x] Header files (`.h`) vs implementation files (`.m`) — why Obj-C splits these and Swift doesn't
- [x] Properties: `@property (nonatomic, strong) NSString *name;` and what `nonatomic`/`strong` mean
- [x] Nullability annotations: `nullable` / `nonnull`
- [x] Swift–Objective-C interop: bridging header, `@objc` attribute, why a company might still have legacy Obj-C modules
- [x] Memory management history: manual retain/release (pre-ARC) vs ARC today

### Block 2 (60 min) — Memory management deep-dive
- [x] ARC (Automatic Reference Counting) — how it counts strong references
- [x] Strong vs weak vs unowned references — definitions and when to use each
- [ ] Retain cycles — classic example: closure capturing `self` strongly, or two objects holding strong refs to each other
- [ ] `weak var delegate` pattern and why delegates are declared weak
- [ ] Deinitializers (`deinit`) — how to confirm an object was deallocated

### Block 3 (75 min) — UIKit fundamentals
- [ ] `UIViewController` lifecycle methods in order: `loadView` → `viewDidLoad` → `viewWillAppear` → `viewDidAppear` → `viewWillDisappear` → `viewDidDisappear`
- [ ] Auto Layout — constraints, `NSLayoutConstraint`, content hugging & compression resistance priority
- [ ] Delegation pattern — protocol + `weak var delegate` + conforming class
- [ ] `UITableView` / `UICollectionView` — data source & delegate protocols, cell reuse via `dequeueReusableCell`, why reuse matters for performance
- [ ] Storyboards vs programmatic UI vs XIBs — trade-offs
- [ ] Segues — what they are, how data is passed via `prepare(for:sender:)`
- [ ] `UINavigationController` and `UITabBarController` — navigation stack basics

### Block 4 (75 min) — SwiftUI deep-dive (your strength — make it airtight)
- [ ] `@State` — local, private, value-type state owned by the view
- [ ] `@Binding` — a reference to state owned by a parent view
- [ ] `@ObservedObject` — observes an external `ObservableObject`, doesn't own its lifecycle
- [ ] `@StateObject` — owns and creates the `ObservableObject` instance (created once per view identity)
- [ ] `@EnvironmentObject` — object injected into the view hierarchy, accessible without explicit passing
- [ ] `ObservableObject` protocol + `@Published` property wrapper
- [ ] View identity & diffing — how SwiftUI decides whether to re-render a view
- [ ] `some View` — opaque return types, why SwiftUI uses them
- [ ] View modifiers — order-dependence (e.g. `.padding().background()` vs `.background().padding()`)
- [ ] Basic Combine — `PassthroughSubject`, `CurrentValueSubject`, `.sink`, how `@Published` ties into Combine

**Sample questions to prep answers for:**
- [ ] "What's the difference between `@StateObject` and `@ObservedObject`?"
- [ ] "How would you cause a retain cycle in Swift, and how do you fix it?"
- [ ] "Walk me through the `UIViewController` lifecycle."
- [ ] "Have you worked with Objective-C? How does it interoperate with Swift?"

---

## WEDNESDAY — 5 hrs — Performance, Concurrency, Architecture, Networking

### Block 1 (75 min) — iOS SDK performance tools & optimization (explicit in JD)
- [ ] Instruments app — what it is, how to launch it (Xcode → Product → Profile)
- [ ] Time Profiler — finds where CPU time is spent, sampling call stacks
- [ ] Allocations instrument — tracks memory allocations, object counts, heap growth
- [ ] Leaks instrument — detects retained objects with no references (memory leaks)
- [ ] Energy Log — tracks battery/CPU/network/location impact
- [ ] Core Animation instrument (FPS) — checking for dropped frames / jank
- [ ] Optimization techniques checklist:
  - [ ] Lazy loading (`lazy var`, loading images/data only when needed)
  - [ ] Image caching (e.g. `NSCache`, or libraries like Kingfisher/SDWebImage)
  - [ ] Reducing view hierarchy depth (fewer nested stacks/views = faster layout passes)
  - [ ] Reusing cells properly in table/collection views
  - [ ] Keeping heavy work off the main thread (main thread only for UI updates)
  - [ ] Avoiding retain cycles (ties back to memory leaks)
  - [ ] Using `Instruments` before optimizing — "measure, don't guess"

### Block 2 (75 min) — Concurrency
- [ ] GCD (Grand Central Dispatch) — queues: serial vs concurrent
- [ ] `DispatchQueue.main` vs background queues — when to use each
- [ ] `DispatchQueue.global(qos:)` — quality-of-service levels: `.userInteractive`, `.userInitiated`, `.utility`, `.background`
- [ ] `async`/`await` — Swift's structured concurrency syntax
- [ ] `Task` — how to launch async work from sync context
- [ ] Actors — how they prevent data races by serializing access to mutable state
- [ ] `@MainActor` — ensuring code runs on the main thread
- [ ] Race conditions — what they are, an example, and how actors/locks/queues prevent them
- [ ] Deadlocks — what causes one (e.g. calling `.sync` on the current queue)

### Block 3 (75 min) — Architecture patterns
- [ ] MVC (Model-View-Controller) — Apple's default pattern, and its "Massive View Controller" problem
- [ ] MVVM (Model-View-ViewModel) — how ViewModel separates business logic from view, why it pairs well with SwiftUI's `@Published`/`ObservableObject`
- [ ] MVVM-C (MVVM + Coordinator) — Coordinator pattern handles navigation logic separately from view controllers
- [ ] Clean Architecture — layering (data/domain/presentation), dependency rule (inner layers don't know about outer layers)
- [ ] Dependency Injection — passing dependencies in via initializer/property instead of creating them internally; why it improves testability
- [ ] Protocol-based dependency injection — using protocols to mock dependencies in tests

### Block 4 (45 min) — Networking & Persistence
- [ ] `URLSession` — `URLSession.shared.dataTask`, async variant `URLSession.shared.data(from:)`
- [ ] `Codable` protocol — `Encodable` + `Decodable`, `JSONDecoder`/`JSONEncoder`
- [ ] REST API basics — HTTP methods (GET/POST/PUT/DELETE), status codes (200, 401, 404, 500)
- [ ] Error handling in networking — `Result<Success, Failure>` type
- [ ] `UserDefaults` — small key-value settings storage, not for sensitive/large data
- [ ] Keychain — secure storage for sensitive data like tokens/passwords
- [ ] Core Data — Apple's object graph & persistence framework, `NSManagedObject`, `NSPersistentContainer`
- [ ] SwiftData — newer declarative persistence framework (Swift-native successor direction to Core Data)
- [ ] When to choose which: UserDefaults (settings) vs Keychain (secrets) vs Core Data/SwiftData (structured relational data)

**Sample questions to prep answers for:**
- [ ] "How would you find a memory leak in an app?"
- [ ] "What's the difference between concurrent and serial queues?"
- [ ] "Why would you choose MVVM over MVC?"
- [ ] "How do you persist data in an iOS app, and what factors decide which storage option you use?"

---

## THURSDAY — 5 hrs — Testing, Process, DSA, Behavioral, Mock Run

### Block 1 (45 min) — Clean code & SOLID
- [ ] Single Responsibility Principle — a class/type should have one reason to change
- [ ] Open/Closed Principle — open for extension, closed for modification
- [ ] Liskov Substitution Principle — subtypes must be substitutable for their base types
- [ ] Interface Segregation Principle — prefer many small protocols over one large one
- [ ] Dependency Inversion Principle — depend on abstractions (protocols), not concrete types

### Block 2 (45 min) — TDD & Testing
- [ ] TDD cycle — Red (write failing test) → Green (make it pass) → Refactor
- [ ] XCTest framework — `XCTestCase`, `XCTAssertEqual`, `XCTAssertTrue`, `setUp`/`tearDown`
- [ ] Unit tests vs UI tests — testing logic in isolation vs testing the full UI flow
- [ ] Mocking/stubbing — creating fake implementations of a protocol dependency to isolate the code under test
- [ ] Test coverage — what it measures, why 100% isn't the real goal (meaningful coverage matters more)

### Block 3 (30 min) — CI/CD & Agile
- [ ] Continuous Integration — automatically building & testing code on every commit/PR
- [ ] Continuous Delivery/Deployment — automatically preparing (or shipping) a release build after CI passes
- [ ] Common iOS CI/CD tools (conceptual, not hands-on): Fastlane (automates builds/screenshots/App Store uploads), Xcode Cloud, GitHub Actions
- [ ] Agile/Scrum vocabulary: sprint, standup, backlog, story points, retrospective, sprint planning
- [ ] Be ready to describe: how your team at 10Pearls used Agile/sprints day to day

### Block 4 (90 min) — DSA / Problem-solving in Swift
Solve each below out loud, explaining approach before coding:
- [ ] Reverse a string / check if a string is a palindrome
- [ ] Find duplicates in an array
- [ ] Two Sum problem (find two numbers in an array that add to a target)
- [ ] FizzBuzz
- [ ] Remove duplicates from an array while preserving order
- [ ] Find the first non-repeating character in a string
- [ ] Basic recursion example: factorial or Fibonacci
- [ ] Explain Big-O of your solutions (e.g. O(n), O(n²)) after solving each

### Block 5 (45 min) — Behavioral (STAR format)
- [ ] Story 1: a time you proactively identified and resolved a project roadblock before it became a bigger problem
- [ ] Story 2: a time you worked cross-functionally with product/design to define and ship a feature
- [ ] Story 3: a time you had to work in a fast-paced, deadline-driven environment
- [ ] Prepare 2–3 questions to ask the interviewer (e.g. about their tech stack, team structure, or how the iOS team collaborates with product across the 16 countries they operate in)

### Block 6 (45 min) — Xcode/tooling odds and ends (small but commonly asked)
- [ ] Build Schemes — what a scheme is (a configuration bundling build settings + target + test/run/archive actions), Debug vs Release configuration
- [ ] Targets — what a target is (a product being built, e.g. app vs extension)
- [ ] `.xcconfig` files — externalized build settings (mention only if asked in depth)
- [ ] Provisioning profiles/signing — a one-line explanation (not deep prep, just don't be blank if asked)

**Sample questions to prep answers for:**
- [ ] "Tell me about a time you resolved a roadblock without being asked."
- [ ] "How do you approach testing a new feature?"
- [ ] "What's a build scheme and why would you use more than one?"
- [ ] Walk through your Two Sum solution and its time complexity.

---

## FRIDAY — Revision only (no new material)

- [ ] Walk the register tab by tab, out loud, answering as if the interviewer asked
- [ ] For each tab: can you answer in under 60 seconds without notes?
- [ ] Re-read your 3 STAR stories once
- [ ] Re-read your 2–3 questions for the interviewer
- [ ] Do NOT introduce new topics today

---

## Full topic coverage check (nothing left out)
- [ ] Swift language & OOP
- [ ] Objective-C
- [ ] UIKit
- [ ] SwiftUI
- [ ] Memory management / ARC
- [ ] Performance tools (Instruments)
- [ ] Optimization techniques
- [ ] Concurrency (GCD, async/await, actors)
- [ ] Architecture patterns (MVC, MVVM, MVVM-C, Clean Architecture)
- [ ] Dependency Injection
- [ ] Networking (URLSession, Codable, REST)
- [ ] Persistence (UserDefaults, Keychain, Core Data, SwiftData)
- [ ] Clean code / SOLID
- [ ] TDD / unit testing / UI testing
- [ ] CI/CD concepts
- [ ] Agile/Scrum vocabulary
- [ ] DSA / problem-solving in Swift
- [ ] Build schemes & targets
- [ ] Behavioral STAR stories
- [ ] Questions to ask the interviewer
