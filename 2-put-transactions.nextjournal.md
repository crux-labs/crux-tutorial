# 2: Put Transactions with Crux – Pluto Assignment

![Clojure_logo.png][nextjournal#file#6e1e3414-7ad4-42fa-ace0-6939985e69e2]

# Introduction

This is the second part of the Crux tutorial. The Earth installment looked at setting up a simple Crux standalone node and a very simple `put` transaction.

## Setup

You need to get Crux running before you can use it.

```edn no-exec id=ffcf0396-b3f9-40e6-a0c2-654401879781
{:deps
 {org.clojure/clojure {:mvn/version "1.10.0"}
  org.clojure/tools.deps.alpha
  {:git/url "https://github.com/clojure/tools.deps.alpha.git"
   :sha "f6c080bd0049211021ea59e516d1785b08302515"}
  juxt/crux-core {:mvn/version "RELEASE"}}}
```

```clojure id=35dc65e9-f458-4e32-9a59-1af72cd12a78
(require '[crux.api :as crux])
```

# Arrival on Pluto

As you enter the Plutonian atmosphere, a message pops up on your communication panel:

```clojure no-exec id=a86efe70-c896-4cc1-ad16-2bdc46cb01b2
"Welcome to the dwarf planet Pluto. You are entering privately governed space. If you do not have the correct papers, entry will be denied. We hope you enjoy your stay.

Have a nice day."

- Anarchic Directorate of Pluto 
```

The government of Pluto is asking to see your flight manifest.

## Choose your path:

**You have your manifest** : *You have permission to land, continue to the space port.*

**You do not have your manifest** : *You do not have permission to land. You can either return to [Earth](https://nextjournal.com/crux-tutorial/getting-started-with-crux), or continue at your own risk.*

# Space Port

As you circle the dwarf planet to land, you have a quick read of your Crux manual. You know you will be using the `put` operation a lot for this assignment and although you used the operation to add your manifest before you left, you think it is a good idea to brush up on your knowledge.

```clojure no-exec id=223ddbe8-8eed-4e69-ae1c-57f484971dcb
"Currently there are only four transaction operations in Crux: put, delete, match and evict.

		Transaction 	(Description)
    put    		(Writes a version of a document)
    delete    (Deletes a version of a document)
    match     (Stops a transaction if the precondition is not met.)
    evict    	(Removes a document entirely)

    
Put:
The put transaction is used to write versions of a document (doc).

Each document must be in Extensible Data Notation (edn) and must contain a unique :crux.db/id value. However, beyond those two requirements you have the flexibility to add whatever you like to your documents because Crux is schemaless.

Along with the document (doc), put has two optional additional arguments:
    start valid-time 	  (The time at which the entry will be valid from.)
		end valid-time 	    (The time at which the entry will be valid until.)

This means that you can query back through the database, you can use valid-time arguments to see the state of the database at a different time.



Time in Crux is denoted #inst "yyyy-MM-ddThh:mm:ss". For example, 9:30 pm on January 2nd 1999 would be written:

    #inst "1999-01-02T21:30:00".


A complete put transaction has the form:
[:crux.tx/put doc valid-time-start valid-time-end]"

— Crux manual
```

*[Read More](https://juxt.pro/crux/docs/transactions.html#_overview)*

You are happy with what you have read, and in anticipation of your first assignment you define the standalone node.

```clojure id=2bdeaaa6-3672-48c1-bbc7-aa5d05fd1153
(def crux (crux/start-node {}))
```

# Assignment

You land on the surface of the dwarf planet. As you do, the job ticket for this assignment is unlocked.

## Ticket

```clojure no-exec id=b62c033a-93f1-438b-abf1-bbbd0050c31f
Task 			"Commodity Logging"
Company		"Tombaugh Resources Ltd."
Contact 			"R. Glogofloon"
Submitted "2115-02-20T13:38:20"
Additional information:
"We need help setting up a new recording system for our mine. I have enclosed a list of the commodities we deal with. Please send someone soon because we already have a week’s worth of unrecorded stocktakes."
```

[commodities.txt][nextjournal#file#48b4711b-6c2d-4b09-9584-a15ef4f05ef2]

You make your way over to the mines on the next shuttle. On your way you decide to get a head start and put the commodities into Crux.

```clojure id=ff24c118-14bf-4e4a-a4d3-03e890292d1b
(crux/submit-tx crux
                [[:crux.tx/put
                  {:crux.db/id :commodity/Pu
                   :common-name "Plutonium"
                   :type :element/metal
                   :density 19.816
                   :radioactive true}]

                 [:crux.tx/put
                  {:crux.db/id :commodity/N
                   :common-name "Nitrogen"
                   :type :element/gas
                   :density 1.2506
                   :radioactive false}]

                 [:crux.tx/put
                  {:crux.db/id :commodity/CH4
                   :common-name "Methane"
                   :type :molecule/gas
                   :density 0.717
                   :radioactive false}]])
```

Since it takes six hours for each transaction to reach your Crux node on Earth from here, it is a good idea to batch up all the commodities in a single transaction.

## Stock Take

You arrive at the mine and are met by the CEO, Reginald Glogofloon, a 150 year old Plutonian.

```clojure no-exec id=a95f32b0-7b7a-4381-8087-68c09f19e100
"Hello, I’m glad you’re here.

I would like you to fill in our last weeks worth of data on our commodities. We need to be able to look back at a given day and see what our stocks were for auditing purposes.

The stock for each day must be submitted at 6pm Earth time (UTC) for your banks records.

Are you able to do that for me?"

— R. Glogofloon 
```

## Choose your path:

**"Yes I'll give it a go"** : *Continue to complete the assignment.*

**"I'm not sure how to even begin"** : *Go back and read the manual entry.*

You remember that with Crux you have the option of adding a `valid-time`. This comes in useful now as you enter the weeks worth of stock takes for Plutonium.

```clojure id=990d9283-0343-4d24-8d59-e5ed9aa04668
(crux/submit-tx crux
                [[:crux.tx/put
                  {:crux.db/id :stock/Pu
                   :commod :commodity/Pu
                   :weight-ton 21 }
                  #inst "2115-02-13T18"]

                 [:crux.tx/put
                  {:crux.db/id :stock/Pu
                   :commod :commodity/Pu
                   :weight-ton 23 }
                  #inst "2115-02-14T18"]

                 [:crux.tx/put
                  {:crux.db/id :stock/Pu
                   :commod :commodity/Pu
                   :weight-ton 22.2 }
                  #inst "2115-02-15T18"]

                 [:crux.tx/put
                  {:crux.db/id :stock/Pu
                   :commod :commodity/Pu
                   :weight-ton 24 }
                  #inst "2115-02-18T18"]

                 [:crux.tx/put
                  {:crux.db/id :stock/Pu
                   :commod :commodity/Pu
                   :weight-ton 24.9 }
                  #inst "2115-02-19T18"]])
```

You notice that the amount of Nitrogen and Methane has not changed which saves you some time:

```clojure id=7fb5e55a-cf65-4628-8079-5597bad38806
(crux/submit-tx crux
                [[:crux.tx/put
                  {:crux.db/id :stock/N
                   :commod :commodity/N
                   :weight-ton 3 }
                  #inst "2115-02-13T18"
                  #inst "2115-02-19T18"]

                 [:crux.tx/put
                  {:crux.db/id :stock/CH4
                   :commod :commodity/CH4
                   :weight-ton 92 }
                  #inst "2115-02-15T18"
                  #inst "2115-02-19T18"]])
```

The CEO is impressed with your speed, but a little skeptical that you have done it properly.

You gain their confidence by showing them the entries for Plutonium on two different days:

```clojure id=87681d10-7b1f-481c-ab36-80dffbf5ecba
(crux/entity (crux/db crux #inst "2115-02-14") :stock/Pu)
```

```clojure id=42e3db39-68e7-4786-b5f5-1e11202a7bd7
(crux/entity (crux/db crux #inst "2115-02-18") :stock/Pu)
```

## Easy Ingest

As a parting gift to them you create an easy ingest function so that if they needed to add more commodities to their stock list they could do it fast.

```clojure id=c0425038-62d2-4117-b3ff-3fa7e2f4700e
(defn easy-ingest
  "Uses Crux put transaction to add a vector of documents to a specified
  node"
  [node docs]
  (crux/submit-tx node
                  (vec (for [doc docs]
                         [:crux.tx/put doc]))))
```

Tombaugh Resources Ltd. are happy that this will be simple enough to use. They thank you for the extra help and you head back to your ship.

# Space Port

You are back at your ship and check your communications panel. There is a new assignment waiting for you:

```clojure no-exec id=efc84bb8-702c-4fa7-82f8-6cfef39dd934
"Congratulations on completing your first assignment.

We would like you to go to Mercury, the hub of the trade world. Their main trade center has a new IT department and want you to show them how to query Crux"

— Helios Banking Inc. 
```

It’s a long flight so you refuel, and update your manifest. You have been awarded a new badge, so you add this to your manifest.

```clojure id=ab2ade6b-7352-4e44-b659-cad259ea12f9
(crux/submit-tx
 crux
 [[:crux.tx/put 
   {:crux.db/id :manifest
    :pilot-name "Johanna"
    :id/rocket "SB002-sol"
    :id/employee "22910x2"
    :badges ["SETUP" "PUT"]
    :cargo ["stereo" "gold fish" "slippers" "secret note"]}]])
```

You enter the countdown for lift off to Mercury. [See you soon.](https://nextjournal.com/crux-tutorial/datalog-queries)

![pluto.png][nextjournal#file#e30d339b-2518-4991-a8a3-2fd589271aa9]


[nextjournal#file#6e1e3414-7ad4-42fa-ace0-6939985e69e2]:
<https://nextjournal.com/data/QmRZrgyeVttQNmkPt7D4fz8kPy8KAoFvahV74kFCkVo8N2?content-type=image/png&node-id=6e1e3414-7ad4-42fa-ace0-6939985e69e2&filename=Clojure_logo.png&node-kind=file>

[nextjournal#file#48b4711b-6c2d-4b09-9584-a15ef4f05ef2]:
<https://nextjournal.com/data/Qmct2hGgaAQDaxMFcdBoKarvzvSPhv384scisZr7V1BEyG?content-type=text/plain&node-id=48b4711b-6c2d-4b09-9584-a15ef4f05ef2&filename=commodities.txt&node-kind=file>

[nextjournal#file#e30d339b-2518-4991-a8a3-2fd589271aa9]:
<https://nextjournal.com/data/QmVS69wN3o1gSj3mKfi1gYKUR1Kzb7tvENAmn7dkPpm36w?content-type=image/png&node-id=e30d339b-2518-4991-a8a3-2fd589271aa9&filename=pluto.png&node-kind=file>

<details id="com.nextjournal.article">
<summary>This notebook was exported from <a href="https://nextjournal.com/a/LPTEfabvNe9tQwAxnjcVb?change-id=Cwhb8NVitzzYpLhtwA4ew3">https://nextjournal.com/a/LPTEfabvNe9tQwAxnjcVb?change-id=Cwhb8NVitzzYpLhtwA4ew3</a></summary>

```edn nextjournal-metadata
{:article
 {:settings nil,
  :nodes
  {"223ddbe8-8eed-4e69-ae1c-57f484971dcb"
   {:id "223ddbe8-8eed-4e69-ae1c-57f484971dcb",
    :kind "code-listing",
    :name "Crux manual"},
   "2bdeaaa6-3672-48c1-bbc7-aa5d05fd1153"
   {:compute-ref #uuid "54ed53cc-4fea-4c57-a18f-2a5af2cb2fa1",
    :exec-duration 65,
    :id "2bdeaaa6-3672-48c1-bbc7-aa5d05fd1153",
    :kind "code",
    :output-log-lines {},
    :refs (),
    :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
   "35dc65e9-f458-4e32-9a59-1af72cd12a78"
   {:compute-ref #uuid "a3d0ea09-8bcf-4964-8e2c-73703f4aa02b",
    :exec-duration 57,
    :id "35dc65e9-f458-4e32-9a59-1af72cd12a78",
    :kind "code",
    :output-log-lines {},
    :refs (),
    :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
   "42e3db39-68e7-4786-b5f5-1e11202a7bd7"
   {:compute-ref #uuid "b65f2ce4-1c81-4443-9fda-aa63e7690144",
    :exec-duration 51,
    :id "42e3db39-68e7-4786-b5f5-1e11202a7bd7",
    :kind "code",
    :output-log-lines {},
    :refs (),
    :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
   "48b4711b-6c2d-4b09-9584-a15ef4f05ef2"
   {:id "48b4711b-6c2d-4b09-9584-a15ef4f05ef2", :kind "file"},
   "6e1e3414-7ad4-42fa-ace0-6939985e69e2"
   {:id "6e1e3414-7ad4-42fa-ace0-6939985e69e2",
    :kind "file",
    :layout :normal},
   "7fb5e55a-cf65-4628-8079-5597bad38806"
   {:compute-ref #uuid "de6c4561-fd3e-42c2-bc2d-d049fb929b07",
    :exec-duration 119,
    :id "7fb5e55a-cf65-4628-8079-5597bad38806",
    :kind "code",
    :output-log-lines {},
    :refs (),
    :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
   "80403b0a-1226-48ff-9bcc-624ed02e3635"
   {:environment
    [:environment
     {:article/nextjournal.id
      #uuid "5b45eb52-bad4-413d-9d7f-b2b573a25322",
      :change/nextjournal.id
      #uuid "5cd52af1-7a79-4804-a169-d6ffcdb6eb7a",
      :node/id "0ae15688-6f6a-40e2-a4fa-52d81371f733"}],
    :id "80403b0a-1226-48ff-9bcc-624ed02e3635",
    :kind "runtime",
    :language "clojure",
    :type :nextjournal,
    :runtime/mounts
    [{:src [:node "ffcf0396-b3f9-40e6-a0c2-654401879781"],
      :dest "/deps.edn"}]},
   "87681d10-7b1f-481c-ab36-80dffbf5ecba"
   {:compute-ref #uuid "a684fb6e-8725-4cf9-90cb-820b0541ba91",
    :exec-duration 73,
    :id "87681d10-7b1f-481c-ab36-80dffbf5ecba",
    :kind "code",
    :output-log-lines {},
    :refs (),
    :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
   "990d9283-0343-4d24-8d59-e5ed9aa04668"
   {:compute-ref #uuid "90de4604-f316-48b8-97e5-76b167af83d8",
    :exec-duration 182,
    :id "990d9283-0343-4d24-8d59-e5ed9aa04668",
    :kind "code",
    :output-log-lines {},
    :refs (),
    :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
   "a86efe70-c896-4cc1-ad16-2bdc46cb01b2"
   {:id "a86efe70-c896-4cc1-ad16-2bdc46cb01b2", :kind "code-listing"},
   "a95f32b0-7b7a-4381-8087-68c09f19e100"
   {:id "a95f32b0-7b7a-4381-8087-68c09f19e100",
    :kind "code-listing",
    :name "First contact"},
   "ab2ade6b-7352-4e44-b659-cad259ea12f9"
   {:compute-ref #uuid "35b4f308-5066-47ef-99b6-8fb4ac8165ba",
    :exec-duration 64,
    :id "ab2ade6b-7352-4e44-b659-cad259ea12f9",
    :kind "code",
    :output-log-lines {},
    :refs (),
    :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
   "b62c033a-93f1-438b-abf1-bbbd0050c31f"
   {:id "b62c033a-93f1-438b-abf1-bbbd0050c31f",
    :kind "code-listing",
    :name "Job Ticket"},
   "c0425038-62d2-4117-b3ff-3fa7e2f4700e"
   {:compute-ref #uuid "34a760aa-0d4b-43c6-b7b0-18e5af683a2b",
    :exec-duration 72,
    :id "c0425038-62d2-4117-b3ff-3fa7e2f4700e",
    :kind "code",
    :output-log-lines {},
    :refs (),
    :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
   "e30d339b-2518-4991-a8a3-2fd589271aa9"
   {:id "e30d339b-2518-4991-a8a3-2fd589271aa9",
    :kind "file",
    :layout :normal},
   "efc84bb8-702c-4fa7-82f8-6cfef39dd934"
   {:custom-language "Communications Panel",
    :id "efc84bb8-702c-4fa7-82f8-6cfef39dd934",
    :kind "code-listing",
    :name "Communications Panel"},
   "ff24c118-14bf-4e4a-a4d3-03e890292d1b"
   {:compute-ref #uuid "4f947055-396d-445e-83d5-802666ce7ee5",
    :exec-duration 96,
    :id "ff24c118-14bf-4e4a-a4d3-03e890292d1b",
    :kind "code",
    :output-log-lines {},
    :refs (),
    :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
   "ffcf0396-b3f9-40e6-a0c2-654401879781"
   {:id "ffcf0396-b3f9-40e6-a0c2-654401879781",
    :kind "code-listing",
    :name "deps.edn"}},
  :nextjournal/id #uuid "02b4f7e7-6b8f-4b53-8f81-d7728c193a66",
  :article/change
  {:nextjournal/id #uuid "60b7b3cb-d296-46e2-ad98-1c671aa255fa"}}}

```
</details>
