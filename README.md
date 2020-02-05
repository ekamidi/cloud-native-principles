# Cloud Native Networking Preamble

**What is a CNF?**

In order to talk about CNFs, we need to define cloud native[1]. Cloud native systems are, among other things, a set of loosely coupled services. These services, also known as microservices, are deployed onto immutable infrastructure while being managed by an orchestrator. This paper includes four links to other papers that go into detail about the definitions of cloud native, microservices, immutable infrastructure, and CNFs from an OSI layer perspective.

**How are cloud native systems loosely coupled?**

Cloud native systems have a clear separation between their processes[2]. These microservices usually use a technology like containers and aim for one process per container3. Cloud native applications also have all of their dependencies packaged with them during the build phase which is then used for deployment4.

**How is immutable infrastructure provisioned?**

Immutable infrastructure (the orchestrator and all of the software and hardware that it depends on) is provisioned using **baked** and **versioned** **templates[5]** (e.g. server images produced from packer[10]) or a combination of templates and **bootstrapping[6]** (some repeatable and versioned process that is applied to the template e.g. Kubeadm). The underlying infrastructure is not changed after it is made ready for use. New changes to the infrastructure are rolled out as new instances of infrastructure.

**How are cloud native systems deployed?**

Cloud native applications are deployed onto immutable infrastructure \(generic host servers that support orchestration[7]\). Cloud native applications are not changed after deployment. New features for an application are rolled out as new artifacts and con\figuration \(e.g. containers\)

**How are cloud native systems configured?**

Cloud native systems are configured declaratively8. This means that the system configuration declares “what” a loosely coupled system should look like, not ”how” the system should be created. The “how” of the application is determined by the tooling \(e.g. the orchestrator, operators, and CRDs\).

**So what is a CNF, actually?**

A CNF is network functionality \(the **implementation** of a network protocol\) that is located in one of the OSI layers[9]. The lower layers \(layers 1 and possibly layer 2\) are provisioned for the higher layers \(2 - 7\) that act as applications deployed onto them. A physical layer 1 networking device should get a ‘flashed’, complete replacement for its artifact updates. The configuration for physical layer 1 is done with an atomic application of a versioned configuration file, which replaces all of the configuration on the device at once. Virtual Layer 1 \(and some of layer 2\) is managed by templated images and bootstrapping while layer 2 and above are managed by an orchestrator.

V2: A CNF is network functionality delivered in software via cloud native development and delivery practices. This functionality lives within the layers of the OSI Model[9] which is used to define a network’s stack. The lower layers \(layers 1 and in some cases layer 2\) are provisioned for the higher layers \(2-7\) to provide transport. These higher layers in this instance act as applications which act upon a network payload \(frames, packets datagrams etc…\). A physical layer 1 networking device should be “flashed” with a complete replacement of its artifacts for updates. The configuration for physical layer 1 is done with an atomic application of a versioned configuration file, which replaces all of the configuration on the device at once. Virtual Layer 1 \(and some layer 2\) is managed via templated images and bootstrapping, while layers 2 through 7 are managed by higher level orchestration and/or an established control plane \(an orchestrator pushing configuration versus a network protocol modifying a route table\).

**Why is this relevant to Service Providers?**

Service providers currently find themselves at a unique transition point within the industry. Their push towards normalization within the world of NFV has finally begun to bear fruit, yet cloud native software approaches are already being pushed by a plethora of vendors, each with their own unique approach. Providers now find themselves in a situation where they must find ways to achieve a return on their investment into NFV while also managing the industry’s shift in paradigm with regards to software development.

**LIST OF CONTRIBUTORS**

If you would like credit for helping with these documents \(for either this document or any of the other four documents linked above\), please add your name to the list of contributors.

W Watson Vulk Coop 

Taylor Carpenter Vulk Coop 

Denver Williams Vulk Coop 

Jeffrey Saelens Charter Communications

## Endnotes

1. "CNCF Cloud Native Definition v1.0", TOC: 2018-06-11, https://github.com/cncf/toc/blob/master/DEFINITION.md“, **Cloud** **native** technologies empower organizations to build and run scalable applications in modern, **dynamic** environments such as public, private, and hybrid clouds. **Containers, service meshes**, **microservices**, **immutable infrastructure,** and **declarative APIs** exemplify this approach. These techniques enable loosely coupled systems that are **resilient**, **manageable**, and **observable**. Combined with robust **automation**, they allow engineers to make high-impact changes frequently and predictably with minimal toil.

2. Stine, Matt. Migrating to Cloud-Native Application Architecture, O'reilly, 2015, pp. 10–11 “_**Codebase**_ Each deployable app is **tracked** as one codebase tracked in **revision** control. It may have many deployed instances across multiple environments. _**Dependencies**_ An app explicitly declares and **isolates dependencie**s via appropriate tooling \(e.g., Maven, Bundler, NPM\) rather than depending on implicitly realized dependencies in its deployment environment. **Config** Configuration, or **anything** that is likely to **differ** between deployment **environments** \(e.g., development, staging, production\) is **injected** via operating system-level **environment** **variables**. _**Backing services**_ Backing services, such as **databases** or message brokers, are treated as **attached resources** and consumed **identically** across all environments. _**Build, release, run**_ The **stages** of building a **deployable** app artifact, **combining** that **artifact** with **configuration**, and **starting** one or more **processes** from that artifact/configuration combination, are strictly **separated**. _**Processes**_ The app executes as one or more **stateless** **processes** \(e.g., master/workers\) that **share** **nothing**. Any necessary state is externalized to **backing** **services** \(cache, object store, etc.\). _**Port binding**_ The app is self-contained and **exports** any/all **services** via **port binding** \(including HTTP\). _**Concurrency**_ Concurrency is usually accomplished by **scaling out app processes horizontally** \(though processes may also multiplex work via internally managed threads if desired\). _**Disposability**_ Robustness is maximized via **processes** that **start up** quickly and **shut down gracefully**. These aspects allow for **rapid elastic scaling**, deployment of changes, and **recovery** from crashes. _**Dev/prod parity**_ Continuous delivery and deployment are enabled by **keeping** **development**, **staging**, and **production** environments as **similar** as possible. _**Logs**_ Rather than managing logfiles, **treat logs as event streams**, allowing the execution environment to **collect**, **aggregate**, **index**, and **analyze** the **events** via **centralized** services. _**Admin processes**_ Administrative or **management tasks**, such as database migrations, are executed as **one-off processes** in environments identical to the app’s long-running processes.”

3. The best way to think of a **container** is as a **method** to **package** a **service**, application, or job. It’s an RPM on steroids, taking the application and adding in its dependencies, as well as providing a standard way for its **host** system to **manage** its **runtime** environment . Rather than a single container running multiple processes, aim for **multiple** **containers**, each running **one** **process**. These processes then become **independent**, **loosely** **coupled** entities. This makes containers a nice match for microservice application architectures. Morris, Kief. Infrastructure as Code: Managing Servers in the Cloud \(Kindle Locations 1708-1711\). O'Reilly Media. Kindle Edition.

4. The benefits of **decoupling** **runtime** **requirements** from the **host** **system** are particularly powerful for infrastructure management. It creates a clean **separation** of concerns between **infrastructure** and **applications**. The host system **only** needs to have the **container** **runtime** **software** installed, and then it can run nearly any container image. Applications, services, and jobs are packaged into containers along with all of their dependencies \[...\]. These dependencies can include operating system packages, language runtimes, libraries, and system files. **Different** **containers** may have different, even **conflicting** **dependencies**, but still run on the **same** **host** without issues. **Changes** to the **dependencies** can be made **without** any **changes** to the **host** system. Morris, Kief. Infrastructure as Code: Managing Servers in the Cloud \(Kindle Locations 1652-1658\). O'Reilly Media. Kindle Edition.

5. The **immutable server pattern** mentioned in “Server Change Management Models” **doesn’t make configuration updates to existing servers**. Instead, changes are made by **building a new server** with the new configuration. With **immutable servers**, **configuration** is **usually** **baked** into the **server template**. When the configuration is updated, a new template is **packaged**. **New instances** of **existing servers** are built from the **new template** and used to **replace** the **older servers**. This approach **treats** **server templates** like **software artifacts**. Each build is versioned and tested before being deployed for production use. This creates a high level of confidence in the consistency of the server configuration between testing and production. **Advocates** of **immutable server**s view making a **change** to the **configuration** of a **production** **server** as **bad** practice, no better than modifying the source code of software directly on a production server. Immutable servers can also **simplify configuration** management, by **reducing** the area of the server that **needs** to be managed by **definition files**. Morris, Kief. Infrastructure as Code: Managing Servers in the Cloud \(Kindle Locations 2239-2247\). O'Reilly Media. Kindle Edition.

6. **Bootstrap Configuration** with **Immutable Servers**: The **purest use** of **immutable servers** is to **bake** everything onto the server **template** and **change nothing**, even when creating server instances from the template. **But some teams have found that for certain types of changes, the turnaround time needed to build a new template is too slow.** **An emerging practice is to put almost everything into the server template, but add one or two elements when bootstrapping a new server.** This might be a **configuration setting that is only known when the server is created, or it might be a frequently changing element such as an application build for testing**. A small development team using continuous integration \(CI\) or continuous delivery \(CD\) is likely to deploy dozens of builds of their application a day, so **building a new server template for every build may be unacceptably slow**. Having a **standard server template image** that can **pull** **in** and **start** a **specified** **application** **build** when it is started is particularly useful for **microservices**. This still follows the **immutable** **server** **pattern**, in that **any** **change** to the server’s **configuration** \(such as a new version of the microservice\) is carried out by **building a new server instance**. It shortens the turnaround time for changes to a microservice, because it **doesn’t** **require** building a **new** **server** **template**. However, this practice arguably **weakens** the **testing** **benefits** from the **pure immutable model**. Ideally, a given **combination** of **server** **template** **version** and **microservice** **version** will have been **tested** through each stage of a change management **pipeline**. But there is some **risk** that the process of installing a microservice, or making other changes, when creating a server will behave slightly differently when done for different servers. This could cause unexpected behavior. So **this practice trades some of the consistency benefits of baking everything into a template and using it unchanged in every instance in order to speed up turnaround times for changes made in this way. In many cases, such as those involving frequent changes, this trade-off works quite well.** Morris, Kief. Infrastructure as Code: Managing Servers in the Cloud \(Kindle Locations 3099-3116\). O'Reilly Media. Kindle Edition.

7. **Containerized services** works by packaging applications and services in **lightweight containers** \(as popularized by Docker\). This **reduces coupling** between **server configuration** and the things that **run on** the **servers**. **So host servers tend to be very simple, with a lower rate of change.** One of the other change management **models** still needs to be **applied** to these **hosts**, but their implementation becomes much simpler and easier to maintain. **Most effort and attention goes into packaging, testing, distributing, and orchestrating the services and applications**, but this follows something similar to the immutable infrastructure model, which again is simpler than managing the configuration of full-blown virtual machines and servers. Morris, Kief. Infrastructure as Code: Managing Servers in the Cloud \(Kindle Locations 1617-1621\). O'Reilly Media. Kindle Edition.

8. **Declarative** **configuration** is **different** from **imperative** **configuration** , where you simply take a series of actions \(e.g., apt-get install foo \) to modify the world. Years of production experience have taught us that maintaining a written **record** of the system’s **desired** **state** leads to a more **manageable**, **reliable** system. Declarative configuration enables numerous **advantages**, including **code** **review** for configurations as well as **documenting** the **current** **state** of the world for distributed teams. Additionally, it is the **basis** for all of the **self-healing** behaviors in Kubernetes that keep applications running **without user action.**” Hightower, Kelsey; Burns, Brendan; Beda, Joe. Kubernetes: Up and Running: Dive into the Future of Infrastructure \(Kindle Locations 892-896\). Kindle Edition.

9. \[...\] **OSI reference model** with up to seven layers, where each layer provides a different level of abstraction and performs a set of well-defined functions. These seven layers are as follows. **1. Physical layer:** These protocols employ methods for bit transmission over physical media and include such typical functions as signal processing, timing, and encoding. **2. Data Link Control \(DLC\) layer:** Its protocols establish point-to-point communication over a physical or logical link, performing such functions as organization of bits in data units \(frames\) organization, error detection, and flow control. **3. Network layer:** These protocols deliver data units over a network composed of the links established through the DLC protocols of layer 2. Part of these protocols is identification of the route the data units will follow to reach their target. **4. Transport layer:** Transport protocols establish end-to-end communication between end systems over the network defined by a layer 3 protocol. Often, transport layer protocols provide reliability, which refers to complete and correct data transfer between end systems. Reliability can be achieved through mechanisms for end-to-end error detection, retransmissions, and flow control. **5. Session layer:** This layer enables and manages sessions for complete data exchange between end nodes. Sessions may consist of multiple transport layer connections. **6. Presentation layer:** This layer is responsible for the presentation of exchanged data in formats that can be consumed by the application layer. **7. Application layer**: The application layer includes protocols that implement or facilitate end-to-end distributed applications over the network.

10. “The **CNF** should **run** **without** **privileges**. **Privileged** actions should be **managed** by the **scheduler** and environment”, x-factor-cnfs, Fred Kautz, [https://www.packer.io/intro/](https://www.packer.io/intro/), “Packer is an open source tool for creating **identical machine images** for **multiple** **platforms** from a single source **configuration**. Packer is lightweight, runs on every major operating system, and is highly performant, creating machine images for multiple platforms in parallel. Packer does not replace configuration management like Chef or Puppet. In fact, when building images, **Packer** is able to use tools like Chef or Puppet to **install** **software** onto the **image.** A _**machine image**_ is a single static unit that contains a **pre-configured operating system** and **installed software** which is used to quickly create new running machines. Machine image formats change for each platform. Some examples include [AMIs](https://en.wikipedia.org/wiki/Amazon_Machine_Image) for EC2, VMDK/VMX files for VMware, OVF exports for VirtualBox, etc..”

