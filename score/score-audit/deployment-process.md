# Deployment Process

This document explains the deploy process of SCORE \(Smart Contract on Reliable Environment\) in the ICON network.

Distribution of SCORE is possible through T-Bears, and DAPP developers will need a wallet to pay the transaction fee. Update of SCORE that has already been distributed is only possible through the same wallet address.

SCORE, which runs on the ICON network, may attack the ICON network intentionally or by mistake. The ICON network adops the SCORE audit process to prevent many from being attacked by the few in advance. SCORE audit is conducted on ICON Mainnet only.

The auditor\(s\) of ICON network conduct pre-investigation on SCORE deployed by DAPP developers to determine risk factors. SCORE can be "accepted" or "rejected," and only SCORE that has been "accepted" by the auditor can operate in the ICON network.

Next is the state diagram of SCORE. If DAPP developers request to deploy the SCORE through T-Bears, the SCORE will be registered as “pending” in the ICON network. It will be installed only after it is accepted by the auditor. If it is determined that it poses a threat to the ICON network, auditor will “reject” the SCORE. Once activated, initial deployer can update the SCORE, and the previous SCORE will remain active while the updated SCORE is in pending status.

