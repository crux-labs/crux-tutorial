# 1: Getting Started with Crux – A Tale of Time and Space

![Clojure_logo.png][nextjournal#file#6e1e3414-7ad4-42fa-ace0-6939985e69e2]

# Introduction

Welcome to the official Crux tutorial, which complements the official [documentation](https://juxt.pro/crux/docs/index.html).

If you’re new to Crux and want to get a practical hands-on guide, you’re in the right place.

## The Story

It’s the year 2115. You have been hired by an inter-planetary bank, Helios Banking Inc. Your task is to travel around the solar system completing assignments using Crux.

You have been given a company spaceship so transport won’t be a problem. Space Customs require all astronauts to complete a flight manifest for every journey. You also have a handy Crux manual with you so you can read up on some background information before you get stuck into each assignment.

Let’s begin.

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

You are now ready for your first assignment, so you head over to the space port.

# Space Port

On entering your spaceship, you notice a flashing blue light on the left of your communications panel. You submit an iris scan to unlock your first message.

```clojure no-exec id=0dfd4496-febc-41b0-962a-0fc4e590368a
"A warm welcome from the Helios family.

For your first assignment we would like you to go to Pluto and help Tombaugh Resources Ltd. set up their stock reporting system. There will be a ticket with more information waiting for you upon your arrival. Find Reginald if you have any questions.

Safe journeys, and don’t forget to fill in your flight manifest. They won’t let you land without one."

— Helios Banking Inc.
```

## Power up

Before you leave you must fill in your flight manifest. To do this, you must first set up a Crux node. You want to get started as quickly as possible so you decide to use Crux’s inbuilt standalone node.

You read the Crux manual entry for the standalone node to make sure this is OK.

```clojure no-exec id=bda0feab-462e-4584-be69-e9e3c23d25f7
"If you want to get up and running with Crux fast, consider using the standalone node. There is a Crux inbuilt standalone node which is the most simple way to start playing with Crux. Bear in mind that this does not store any information beyond your session.

For persistent storage consider using RocksDB and for scale you should consider using Kafka."

— Crux manual
```

*[Read More](https://juxt.pro/crux/docs/configuration.html#_standalone_system)*

You decide this is fine for now, and so define your Crux node.

```clojure id=2bdeaaa6-3672-48c1-bbc7-aa5d05fd1153
(def crux
  (crux/start-node {}))
```

## Flight Manifest

You take a look around your ship and check the critical levels.

You read the manual entry for putting data into Crux.

```clojure no-exec id=ca575f8d-8096-48d8-acef-9a05712e43c6
"Crux takes information in document form. Each document must be in Extensible Data Notation (edn) and each document must contain a unique `:crux.db/id` value. However, beyond those two requirements you have the flexibility to add whatever you like to your documents because Crux is schemaless."

— Crux manual
```

*[Read More](https://juxt.pro/crux/docs/get_started.html#_schemaless)*

Just as you’re about to write your manifest, one of the porters passes you a secret note and asks you to deliver it to a martian named Kaarlang. They are certain you will meet Kaarlang on your travels and so you see no harm in delivering the note for them.

```clojure id=4a6c4961-14b1-4ac9-96e7-7aec033b55a8
(def manifest
  {:crux.db/id :manifest
   :pilot-name "Johanna"
   :id/rocket "SB002-sol"
   :id/employee "22910x2"
   :badges "SETUP"
   :cargo ["stereo" "gold fish" "slippers" "secret note"]})
```

You put the manifest into Crux.

```clojure id=bd1e9012-f10e-4bfb-bee3-83abf3be162e
(crux/submit-tx crux [[:crux.tx/put manifest]])
```

This is `put`, one of Crux’s four transaction operations. Check that this was successful by asking Crux to show the whole entity.

```clojure id=3e0d81c5-2598-432f-9f83-038b47b5f5fc
(crux/entity (crux/db crux) :manifest)
```

*Note: You should run this code block separately otherwise you may see only `nil` returned. The reason for this is covered in the [await-tx](https://nextjournal.com/crux-tutorial/await/) tutorial.*

You enter the countdown for lift off to Pluto. [See you soon](https://nextjournal.com/crux-tutorial/put).

![pluto.png][nextjournal#file#e30d339b-2518-4991-a8a3-2fd589271aa9]


[nextjournal#file#6e1e3414-7ad4-42fa-ace0-6939985e69e2]:
<https://nextjournal.com/data/QmQSb6p1iZ3MXE9Um2sHMK12ViXgrSMyS4dRmPBpJjSPBR?content-type=image/png&node-id=6e1e3414-7ad4-42fa-ace0-6939985e69e2&filename=Clojure_logo.png&node-kind=file>

[nextjournal#file#e30d339b-2518-4991-a8a3-2fd589271aa9]:
<https://nextjournal.com/data/QmNTBZyiGLZSxz5AY1dhNW6b7GXvw6dnzAAysQBui4TPyy?content-type=image/png&node-id=e30d339b-2518-4991-a8a3-2fd589271aa9&filename=pluto.png&node-kind=file>

<details id="com.nextjournal.article">
<summary>This notebook was exported from <a href="https://nextjournal.com/a/LLjFB11JxUUKUYWtbsGd4?change-id=CwhJPtmvVCmfK5wWTcyP6R">https://nextjournal.com/a/LLjFB11JxUUKUYWtbsGd4?change-id=CwhJPtmvVCmfK5wWTcyP6R</a></summary>

```edn nextjournal-metadata
{:article
 {:settings nil,
  :nodes
  {"0dfd4496-febc-41b0-962a-0fc4e590368a"
   {:id "0dfd4496-febc-41b0-962a-0fc4e590368a", :kind "code-listing"},
   "2bdeaaa6-3672-48c1-bbc7-aa5d05fd1153"
   {:compute-ref #uuid "5c437de6-f597-4daf-b298-b33efc200c51",
    :exec-duration 76,
    :id "2bdeaaa6-3672-48c1-bbc7-aa5d05fd1153",
    :kind "code",
    :output-log-lines {},
    :refs (),
    :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
   "35dc65e9-f458-4e32-9a59-1af72cd12a78"
   {:compute-ref #uuid "316fd86f-c914-435e-b7ce-3c527c97c8bc",
    :exec-duration 54,
    :id "35dc65e9-f458-4e32-9a59-1af72cd12a78",
    :kind "code",
    :output-log-lines {},
    :refs (),
    :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
   "3e0d81c5-2598-432f-9f83-038b47b5f5fc"
   {:compute-ref #uuid "ec370f8f-cff5-4cc8-b3f3-87d93f73032e",
    :exec-duration 57,
    :id "3e0d81c5-2598-432f-9f83-038b47b5f5fc",
    :kind "code",
    :output-log-lines {},
    :refs (),
    :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
   "4a6c4961-14b1-4ac9-96e7-7aec033b55a8"
   {:compute-ref #uuid "4108884b-d4e8-4053-a2a9-afc97268bf21",
    :exec-duration 67,
    :id "4a6c4961-14b1-4ac9-96e7-7aec033b55a8",
    :kind "code",
    :output-log-lines {},
    :refs (),
    :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
   "6e1e3414-7ad4-42fa-ace0-6939985e69e2"
   {:id "6e1e3414-7ad4-42fa-ace0-6939985e69e2",
    :kind "file",
    :layout :normal},
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
   "bd1e9012-f10e-4bfb-bee3-83abf3be162e"
   {:compute-ref #uuid "15aa0cea-9aaa-460d-b8fd-d550c6edc86e",
    :exec-duration 56,
    :id "bd1e9012-f10e-4bfb-bee3-83abf3be162e",
    :kind "code",
    :output-log-lines {},
    :refs (),
    :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
   "bda0feab-462e-4584-be69-e9e3c23d25f7"
   {:id "bda0feab-462e-4584-be69-e9e3c23d25f7", :kind "code-listing"},
   "ca575f8d-8096-48d8-acef-9a05712e43c6"
   {:id "ca575f8d-8096-48d8-acef-9a05712e43c6", :kind "code-listing"},
   "e30d339b-2518-4991-a8a3-2fd589271aa9"
   {:id "e30d339b-2518-4991-a8a3-2fd589271aa9",
    :kind "file",
    :layout :normal},
   "ffcf0396-b3f9-40e6-a0c2-654401879781"
   {:id "ffcf0396-b3f9-40e6-a0c2-654401879781",
    :kind "code-listing",
    :name "deps.edn"}},
  :nextjournal/id #uuid "02b34a27-0a82-4076-85ef-c23ed21471d7",
  :article/change
  {:nextjournal/id #uuid "60b78648-51a6-415d-af80-3afca65102c2"}}}

```
</details>
