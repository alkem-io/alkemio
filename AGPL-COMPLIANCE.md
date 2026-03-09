# Alkemio & AGPL Software: A Position on Compatibility and Compliance

**Version:** 1.0  
**Date:** 2025-06-17

## 1. Introduction

This document outlines the official position of Alkemio B.V. regarding the use of third-party, open-source software containers licensed under the GNU Affero General Public License, version 3.0 (AGPL v3.0).

The Alkemio platform is a complex, container-based SaaS project licensed under the [European Union Public Licence v1.2 (EUPL-1.2)](https://github.com/alkem-io/alkem-io/blob/master/LICENSE). Our architecture leverages a variety of open-source components, some of which are licensed under AGPL v3.0. A notable example is [Element Synapse](https://github.com/element-hq/synapse), a Matrix homeserver implementation.

This document establishes that Alkemio's use of unmodified AGPL-licensed software in containers is fully compliant with the terms of both licenses, without imposing reciprocal AGPL obligations on the core Alkemio codebase.

## 2. Core Position

Alkemio can integrate and use unmodified, containerized AGPL-licensed software without creating new licensing obligations for the Alkemio platform. This position is based on two key pillars:

1.  **Architectural Separation**: The Alkemio platform interacts with AGPL-licensed containers as separate, independent programs communicating over network protocols. This does not create a "derivative" or "combined" work under the definitions of the AGPL.
2.  **Explicit License Compatibility**: The EUPL-1.2 is explicitly compatible with the AGPL v3.0, as confirmed by the European Commission.

## 3. Analysis

### 3.1. Architectural Separation

A primary obligation of the [AGPL v3.0 license](https://www.gnu.org/licenses/agpl-3.0.en.html) is the requirement to provide the source code of any *modified* version of the software to users who interact with it over a network.

This obligation, however, is not triggered in our use case for two reasons:
*   **No Modification**: The Alkemio platform uses official, unmodified container images of AGPL-licensed software. We do not alter their source code.
*   **Independent Programs**: AGPL-licensed containers run as separate processes in our Kubernetes environment. Communication between Alkemio's EUPL-1.2 licensed containers and the AGPL-licensed containers occurs via standard, at-arm's-length network protocols.

This form of interaction between distinct, containerized programs does not create a single, derivative work. The programs are loosely coupled and communicate in a way that is common for modern microservice architectures. Therefore, the license of one independent component does not extend to the other.

For example, our use of Element Synapse follows this pattern: the Synapse container runs independently, and Alkemio communicates with it through the Matrix protocol, maintaining clear architectural boundaries.

### 3.2. EUPL-1.2 and AGPL v3.0 License Compatibility

The EUPL-1.2 was specifically designed for interoperability with other popular open-source licenses. The European Commission, the steward of the EUPL, provides an official [Matrix of EUPL compatible open source licences](https://interoperable-europe.ec.europa.eu/collection/eupl/matrix-eupl-compatible-open-source-licences).

This document explicitly states the compatibility between the two licenses:

> The AGPL is also included in the EUPLv1.2 downstream compatibility list (EUPL Appendix) - therefore the EUPL is compatible with the AGPL: you may distribute under the AGPL a larger derivative work integrating components covered by the EUPL and by the AGPL.

This one-way compatibility is crucial. It means that while a combined EUPL and AGPL work *could* be licensed under the AGPL, the EUPL itself does not force this outcome. Given our architectural separation, the question of relicensing the entire Alkemio platform under AGPL is moot. The compatibility, however, provides a clear and officially sanctioned acknowledgment that the two licenses can coexist within a larger project.

## 4. Alkemio's Obligations & Compliance

Alkemio fully acknowledges and complies with its obligations under the AGPL v3.0 for any AGPL-licensed components it uses:

1.  **Preservation of Notices**: We preserve all original copyright notices and license files within the AGPL-licensed container images we deploy.
2.  **Source Code Provision**: We provide a clear and accessible way for users to obtain the original, unmodified source code for the specific versions of AGPL-licensed software we use. Direct links are provided in our public documentation to the official repositories of these components.
3.  **No Modification**: We use AGPL-licensed software as-is, without altering their source code.

For example, for Element Synapse, we provide a direct link in our documentation to the official [Element Synapse GitHub repository](https://github.com/element-hq/synapse).

## 5. Conclusion

The Alkemio platform's use of AGPL v3.0 licensed software containers is well-considered and compliant with all relevant open-source licensing terms. Our containerized architecture ensures that the AGPL and EUPL components remain distinct works, preventing license entanglement. The explicit compatibility of the EUPL-1.2 with the AGPL v3.0, as stated by the European Commission, further reinforces our position. We are confident that we can continue to leverage powerful, open-source tools—such as Element Synapse and other AGPL-licensed components—without compromising our own licensing or creating undue obligations for the Alkemio project.
