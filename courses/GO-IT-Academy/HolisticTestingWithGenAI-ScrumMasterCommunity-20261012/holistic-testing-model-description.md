# Holistic Testing Model

In January 2021, I wrote a blog post called “Testing And Coding, Not Coding ‘Then’ Testing” to emphasize that testing and coding are part of the same process. In that post, I introduced a diagram (Figure 1) to show continuous testing.

## Continuous testing

I was ok with the diagram but not completely happy with it, and it seemed to confuse some people, so I’ve been tweaking it and considering what message I really wanted to get across and have settled on this diagram / model (Figure 2) to show how I view a development life cycle.

## Dev cycle1

It is similar to the DevOps cycle, but you’ll notice it does separate coding and testing as separate stages within the cycle. To me, the building stage includes coding and all that goes with it, and testing is performed in each and every stage.

It is hard to include feedback cycles in a diagram without complicating it too much, but this cycle is not meant to be framed by time. Some of the stages will go very quickly or maybe very slowly depending on how many knowns or unknowns exist. Sometimes a team will need to pause and go back a stage or two.

## Dev cycle2 with pauses

In Figure 3, I added possible pauses, allowing time for the team to reflect when necessary. For example, perhaps when you are trying to understand more about the feature, you realize that you don’t have enough information and need to go back to planning or discovery? Does this feature even make sense?  Or maybe you’ve deployed to the test environment and you have found some serious issues and need to go back to build, or maybe even back further to understand the behavior or technical implementation a bit more.

I’ve labeled this model “holistic testing”, but so far, I haven’t even mentioned testing. So where does the testing come into play in this model? Taking the idea of Dan Ashby’s “we test here and here and here”, diagram, I’ve tried to list some of the types of testing activities that might happen throughout the cycle (Figure 4). Note: this is not meant to be an exhaustive list.

## Dev cycle3 with tests (Holistic Testing)

The left side of the loop shows the testing we can be done early. The right side of the loop is about testing to find bugs and learn. There is no start or end of an infinite loop, but I’ll start at discovery – the beginning of an idea.

### 1-Discover: 
We are looking for business value. This is often product managers working with customers to decide if the feature is worth while pursuing. They are testing the idea.

#### Test the ideas
- ChatGPT & Co
    - NA
- RBI AI Native SDLC (prefered)
    - start-prd 
        - Input: I have a web-shop to sell tools. I want to expand to the Czech market and want to find out if this makes sense?
    - refine-prd
        - Input: NA
    - ChatGPT: 
        - In V7 the shop should be available for the Chinese market and the Baidu platform. Test the idea with ChatGPT and make a suggestion which market would be easier to go to

#### Determine value
- ChatGPT & Co
    - NA
- RBI AI Native SDLC
    - NA

### 2-Plan: 
Identify the risks to determine which quality attributes will be important to the feature. Features are sliced into smaller stories and this is time to advocate for testable stories. Teams may start looking at high level acceptance tests and start to get a better understanding of the stories.

#### Identify risks
- ChatGPT & Co
    - NA
- RBI AI Native SDLC  (prefered)
    - start-prd
        - Input: 2-Existing documents or specifications
        - Input: Any format..... (I have first draft of requirement documents in the directory sprint6-input)
        - Q: What inputs do you have for this PRD?
        - A: Existing document or specification
        - Q: Please paste or describe your input material here.
        - A: I have first draft of requirement documents in the directory sprint6-input   
    - refine-prd
        - Now I need answers to resolve the remaining open decisions before detailing the BACs:
        - Q: Which solution approach do you want to proceed with?
        - A: Option C: Layered — Transloco for static UI, API for dynamic content
        - Q: US2450 and US4200 describe conflicting delivery cost models. Which is the authoritative spec?
        - A: US4200 (weight-based, CZK/USD, Zasilkovna for CZ+DACH)
        - Q: Should the selected Czech language persist across browser sessions or only within the current session?
        - A: Persistent via browser cookie (no login required)
        - Q: Resolve the remaining open decisions (select all that apply)
        - A: Alza.cz marketplace: defer to next sprint, PayU region check: billing address only, PayU region check: shipping address only, PayU region check: either billing OR shipping is CZ, Czech fallback language: English, Czech fallback language: German
    - ChatGPT: review the risks from prd.md with GenAI

#### Test assumptions
- ChatGPT & Co
    - NA
- RBI AI Native SDLC
    - NA

#### Create testable stories
- ChatGPT & Co
    - NA
- RBI AI Native SDLC
    - NA

### 3-Understanding: 
Practices like acceptance test-driven development or behaviour-driven development are part of understanding the stories. Teams may use user experience experts to learn about the customer experience. Matt Wynne’s example mapping is a great framework to help map business rules to examples, or perhaps teams can use the 7 Product Dimensions from Ellen Gottesdiener and Mary Gorman’s Discover to Deliver for structured conversations. Programmers may help by creating low-fidelity prototypes for fast, iterative feedback to reduce some of the risk around unknowns. Conversations around the different quality attributes let teams determine the ‘what and how’ to instrument the code for testing or monitoring or what events to tag for observing later.

#### ATDD/BDD
- ChatGPT & Co
    - NA
- RBI AI Native SDLC
    - Implement Sprint7-delivery feature
    - start-prd
        - existing document or specification
        - Q: Please paste or describe your existing document or specification. Any format is fine — full text, bullet points, or a file path/URL to the document in this workspace.
        - A: sprint7-delivery-feature
    - refine-prd
        - DONE
    - plan-prd
        - DONE 
    - build-task
        - specify-task 1-4
    - ChatGPT: 
        - Review the business rules from story 1 - Delivery Calculation Engine in a 4-Amigo session
        - Design a set of T1T5 for the business rules

#### Prototypes
- ChatGPT & Co
    - NA
- RBI AI Native SDLC
    - NA

#### Example mapping
- ChatGPT & Co
    - asdfsd
- RBI AI Native SDLC
    - fsfsad

#### Determine what to observe or monitor
- ChatGPT & Co
    - NA
- RBI AI Native SDLC
    - NA

### Building: 
This is where the team implement user stories and features – perhaps practicing test-driven development (TDD) and using fast feedback principles to test as soon as possible. Small testable stories make these practices easier to apply. Ensemble work, pairing, code reviews and test reviews take place. The examples created earlier can be used to create executable tests that guide development. These automated tests can become part of your regression suite to give fast feedback to detect regression failures. Teams will perform exploratory testing to uncover what they didn’t think about when they were developing. Understanding the version control system and workflows help determine what testing needs to be done to manage technical risk.

#### Automate tests
- ChatGPT & Co
    - asdfsd
- RBI AI Native SDLC
    - fsfsad
#### Instrument the code
- ChatGPT & Co
    - asdfsd
- RBI AI Native SDLC
    - fsfsad
#### Test stories & features
- ChatGPT & Co
    - asdfsd
- RBI AI Native SDLC
    - fsfsad


### Deploying: 
“Build once, deploy many.” I remember when I first heard this idea and how it helped the testing effort. Deploying the same build to multiple environments in the deployment pipeline, enables different testing activities to be performed on different environments, knowing the code will always be the same. For example, the differences might be the configuration, or the amount of data used. Many teams still struggle to have stable test environments, but as organizations adopt virtual and cloud infrastructures, teams can spin up test environments on demand. This may make testing some of the quality attributes simpler like load or performance testing. Deploying to a production like environment if you are not yet doing continuous delivery or deployment enables you to test the system as a whole.

#### Run automated tests
- ChatGPT & Co
    - asdfsd
- RBI AI Native SDLC
    - fsfsad
#### Test quality attributes
- ChatGPT & Co
    - asdfsd
- RBI AI Native SDLC
    - fsfsad
#### Test the system
- ChatGPT & Co
    - asdfsd
- RBI AI Native SDLC
    - fsfsad

### Releasing: 
There are many different mechanisms for releasing to the customer these days. If you are practicing continuous delivery or deployment, you may be using a blue / green deployment strategy. Both environments are the same configuration, but the delivery is to the idle one so testing can happen safely. When ready, the switch is flipped, and the new feature is now on the active production site. More and more teams are using release feature toggles which enable a team to put the feature in production but only make it visible to customers when they feel ready.

#### Test in production
- ChatGPT & Co
    - asdfsd
- RBI AI Native SDLC
    - fsfsad
#### User features toggles or blue-green deployment
- ChatGPT & Co
    - asdfsd
- RBI AI Native SDLC
    - fsfsad

### Observability: 
The team captures events they hope will enable them to find problems very quickly if they occur. It’s a way to watch how the customers use the product and allow the team to respond accordingly. Teams can also monitor the system for warnings and errors (if the code has been instrumented to do that).

#### How do customer use the product
- ChatGPT & Co
    - asdfsd
- RBI AI Native SDLC
    - fsfsad
#### Monitor for warnings and error
- ChatGPT & Co
    - asdfsd
- RBI AI Native SDLC
    - fsfsad

### Learning: 
People have questioned me about testing as part of learning. As teams observe how their product is used by their customers, they can hypothesize about how to improve. They are testing our assumptions.

#### Hypothesize and adapt
- ChatGPT & Co
    - asdfsd
- RBI AI Native SDLC
    - fsfsad

## Summary

The term continuous testing has been used and abused, so I’m staying away from that particular term, and I hope that the term “holistic testing” resonates with people. When we test, we need to consider all types of testing, not only the ones we think a tester is responsible for. It includes automation, exploratory testing, or any other type of human-centric testing. It involves the whole team, the product organization, and even the customer. We need to consider testing from a holistic point of view, and I’m hoping this diagram will help people to understand the ‘when’ the different types of testing might take place.

The highest performing teams and organizations that I have worked with, have taken a holistic point of view to the quality of their product, and understand how testing can support that vision.