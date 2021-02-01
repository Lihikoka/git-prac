Agenda Item 8.4.2: Enhancements on UL Time And Frequency Synchronization
===
###### tags: `NR` ,`NTN`
This page is a summary of the email discussion thread on agenda item 8.4.2, mainly in RAN1#104-e.

:::danger
架構應改為：
- Issue#1
    - Issue#1-1
        - Background
        - :::success
            **Agreement in RAN1#103-e**
            
            [Content]
            :::
        - :::info
            **Initial proposal**

            [Content]
          :::
        - Comments from companies
        - :::warning
            **FL recommendation**

            [Content]
            :::
        - Comments from companies
    - Issue#1-2:
        - ...
- Issue#2
    - ...
:::

[TOC]

---

# RAN1#104-e

## Issue#1: Initial acuisition of TA before PRACH preamble transmission

### Issue#1-1: Indication of common TA (CTA)
:::success
**Agreement in RAN1#103-e**

![](https://i.imgur.com/qKHiiB4.png)
:::
- We need to provide more details about the common TA component X in the above proposal: its value, its unit and granularity.
- W.r.t the value of X, a general assumtion within the TDocs is that the CTA is equal to the RTD on the feeder link.
    - The RP is located at the gNB/GW.

:::warning
**Moderator's view**

The exact location of the RP should be an internal matter to the network and therefore the common TA would be determined and broadcast by the gNB. As stated by [Ericsson] the CTA is a network-controlled common TA to compensate (e.g.) for feeder link RTT.
:::

:::info
**Different views of the location of X**

![](https://i.imgur.com/OONOTbi.png)
:::

:::info
**Initial proposal 1-1**

$$
T_{TA} = (N_{TA}+N_{TA,UE-specific}+N_{TA,common}+N_{TA,offset} )×T_c
$$

where:
$N_{TA}$ and $N_{TA, offset}$ are defined as in Release-16.
$N_{TA,UE-specific}$ is UE-autonomous TA to compensate for the service link RTT.
$N_{TA,common}$ is network-controlled common TA.
$T_c$ is specified in TS 38.211 section 4.1.
:::
- Most companies support this proposal.
- Comments from the companies not supporting this proposal:
    - [CATT] We need an agreement for $N_{TA, common}$.
    - [CATT, QC, Xiaomi] ==Signalling overhead may be large.==
    - [Nokia] we are opposing to the definition of $N_{TA,UE-specific}$ as it is directly referring to “compensate for the service link RTT”.

The main concern is the potentially large signalling overhead.

---

#### ==Updated proposal based on company views==
18 over 24 companies support the **Initial proposa 1-1**.

Some concerns are:
- [CATT, Qualcomm, Xiaomi] The granularity of Tc would cost much signalling bits.
    - Moderator:
        - [Thales-R1-2100520] 18 or [ZTE-R1-2100245] 19 bits are required for the signalling bits.
        - Granularity should be proportional to SCS: $16 \cdot 64 / 2^\mu T_c$.
- [CATT] The common TA does not need very accurate indication.
    - Moderator: $T_{TA}$ and all related components including $N_{TA, common}$ should be calculated with sufficient accurary (the acceptable error is yet-to-be defined by RAN4).
- [CMCC] X may be time-varying with continuous value.
    - Moderator: The common TA can be approximated by a linear function as described by Ericsson in [R1-2100927].
- [Apple] The relationship between this common TA and the agreed "common timing offset value" needs to be clarified.
    - Moderator: both are referring to the same RTT.

:::info
**Updated proposal 1-2**

$$
T_{TA} = (N_{TA}+N_{TA,UE-specific}+N_{TA,common}+N_{TA,offset} )×T_c
$$

where:
$N_{TA}$ and $N_{TA, offset}$ are defined as in Release-16.
$N_{TA,UE-specific}$ is UE self-estimated TA.
$N_{TA,common}$ is network-controlled common TA.
$T_c$ is specified in TS 38.211 section 4.1.
:::
- [Xiaomi] prefers to express X in ms to reduce the signalling overhead.


### Issue#1-2: The need and indication of common TA drift rate
:::info
**Potential proposal 1-2** (with removal of "may" in initial proposal 1-2)

The gNB shall broadcast the common TA drift rate as part of the common TA indication
:::
- Most companies agree.
- However, some companies [CATT, OPPO, QC, Spreadtrum, APT] think that more discussions are needed:
    - [OPPO]
        - Whether the drift is a linear funcion?
        - How to ensure the TA variation is monotonic?
        - The value of the drift itself is time varying or invariant? We do not prefer the UE to frequently read system information to get updated drift value. 
        - Would it be more efficient for the network to handle the feeder link drift than for the UE to handle?
- [Panasonic]
    - Even with the common timing drift rate, the accurate compensation might not be possible.
    - DL/UL difference due to the feeder link delay could be managed by gNB implementation to some extent.

---

#### ==Updated proposal based on company views==
- There is only one clear objection [Panasonic].
- More discussions are needed [CATT, OPPO, QC, Spreadtrum, APT].
- Moderator: Many companies provided inputs on the need and benefit of indicating a drift rate [Ericsson- R1-2100927].
    - Timing drift rate was already discussed in the SI and [38.821] states that the indication of timing drift rate from the network to UE is also supported to ebable the TA adjustment at UE side.


:::info
**Updated proposal 1-2**

The gNB shall broadcast the common TA drift rate as part of the common TA indication.
:::



### Issue#1-3: The need and the indication of TA margin
#### Issue#1-3-1: The need of  TA_margin to account for the TA estimation uncertainty
It seems needed at least from RAN1 viewpoint.


#### Issue#1-3-2: Indication of the TA_margin to the UE
The focus should be now on how the TA margin should be indicated to the UE

:::warning
**FL recommmendation in RAN1#103-e**

Regarding the indication of TA margin used to account for TA estimation uncertainty when applying the TA pre-compensation in initial access, companies are encouraged to analyze the pro and cons of the two identified solutions:
- TA margin is indicated in SIB
- TA margin is included within the Common TA. i.e.; Common TA configuration absorbs the maximum TA uncertainty

The value of TA margin will be defined after the specification of  UL time synchronization requirement.
:::

:::info
**Initial proposal 1-3**

The NTN UE calculates its TA as follows:

$$
T_{TA} = (N_{TA}+N_{TA,UE-specific}+N_{TA,common}[-N_{TA, margin}]+N_{TA,offset} )×T_c
$$

N_(TA,margin) is a margin for maximum estimation error of the UE-specific TA, whether it is included within the common TA or explicitly indicated in SIB is FFS
:::
- Half of the companies support this proposal.
- The other companies not supportive of this proposal think there is no need to indicate TA margin. CTA is enough.
- [Apple, MTK] says that the TA margin can also be pre-defined in spec, e.g., 38.211.

---
##### ==Updated proposal based on campny views==
- Moderator: Although the views are still split, TA matgin seems needed depending on requirements for UE-autonomous TA error, PRACH preamble format, common TA error, and common timing drift rate error.

:::info
**Updated proposal 1-3**

For UE with Autonomous acquisition of the TA, to handle the UE’s estimation uncertainty, UE shall use a margin when applying the TA pre-compensation.
:::

#### Issue#1-3-3: The value of TA_margin
To be discussed later.

### Issue#1-4: TA command in RAR
[CMCC, CATT] proposed to withdraw the following working assumption.
:::warning
**Working assumption made in RAN1#103-e**

It is assumed that the requirement on UL time pre-compensation for Msg1/MsgA transmission of an NR NTN UE in idle/inactive mode will be defined such that the existing TAC 12-bit field in msg2 (or msgB) can be reused without any extension.
:::

The intention of the working assumption on TA command in RAR is to have as design target not extending existing TAC 12-bit field in Msg2.

:::warning
**FL Recommendation**

The following working assumption is still valid:

It is assumed that the requirement on UL time pre-compensation for Msg1/MsgA transmission of an NR NTN UE in idle/inactive mode will be defined such that the existing TAC 12-bit field in msg2 (or msgB) can be reused without any extension.
:::
- Most companies agree.

:::info
**Potential Proposal 1-4**

Confirm the following working assumption:

It is assumed that the requirement on UL time pre-compensation for Msg1/MsgA transmission of an NR NTN UE in idle/inactive mode will be defined such that the existing TAC 12-bit field in msg2 (or msgB) can be reused without any extension.
:::

---
#### ==Updated proposal based on company views==
- [ZTE] proposed to postpone the confirmation.
- [Ericsson] So far, nothing has been presented that justifies a change to the TAC format in RAR.
:::info
**Updated proposal 1-4**

Confirm the following working assumption:
It is assumed that the requirement on UL time pre-compensation for Msg1/MsgA transmission of an NR NTN UE in idle/inactive mode will be defined such that the existing TAC 12-bit field in msg2 (or msgB) can be reused without any extension.
:::


## Issue#2: TA update in connected mode
### Issue#2-1: UE capability of TA acquisition in RRC Connected state

:::success
**Agreements in RAN1#103-e**

![](https://i.imgur.com/20JAqe4.png)
:::
- [Qualcomm, Ericsson, Apple, Panasonic, Intel] proposed that the "NO" in the agreement should be be discussed.

:::success
**Agreement**

An NTN UE in RRC_CONNECTED state is required to support UE specific TA calculation based at least on its GNSS-acquired position and the serving satellite ephemeris.
FFS: Operation of closed loop and open loop TA control
:::

### Issue#2-2: TA maintenance
:::info
**Proposal 2-2-1**

For TA update in RRC_CONNECTED state, combination of both open ( i.e. UE autonomous TA estimation, and common TA estimation) and closed (i.e., received TA commands) control loops  shall be supported for NTN.
:::
Most companies support this proposal. However, Nokia prefer closed-loop TA update and the original comment is:

:::warning
**Comment from Nokia**

We prefer network-controlled closed-loop TA update. If the UE further applies open-loop TA update, the network must know the details of the open loop adjustment in advance. Self adjustment by the UE based on GNSS time and the time provided by referenceTimeInfo-R16 suffers less from satellite movement as the gNB reference is a static point, and should be supported as well.
:::

#### ==Update of TA component controlled by Closed loop==
The TA applied by an NR NTN UE is given by:
$$
(N_{TA}+N_{TA,UE-specific}+N_{TA,common}+N_{TA,offset} )×T_c
$$
- $N_{TA}$: closed loop
- $(N_{TA,UE-specific}+N_{TA,common})$: open loop

The TAC provided in Msg2 is an **absolute** timing advance, whereas the subsequent TAC provided within the MAC CE are relative.

:::info
**Solution#1** proposed by FL

![](https://i.imgur.com/u24G38a.png)
:::

> 不懂為什麼會說 "With exception that the TAC provided in mgs2 and subsequent TACs provided within the MAC CE are relative"，proposal 明明就跟現有的 spec 一樣。
> [color=#e85f63][name=Daniel][time=Thu, Jan 28, 2021 10:45 AM]
- Most companies agree with solution#1, whereas OPPO, Sony, [Indian], Nokia do not agree.
- XiaoMi says that DCI approach should also be considered.

---

#### ==Update of TA component controlled by Open loop==
:::info
**Solution#1** proposed by FL

![](https://i.imgur.com/RdeKcoA.png)
:::

- Many companies say that the drift rate for UE-specific TA is not needed.
- QC says that the need of common timing drift rate is unclear.

### Issue#2-3: TA acquisition during Handover
- RACH-less HO in NTN is proposed by [Mitsubishi] and [Ericsson].
- Moderator view: RACH-less HO for NTN will need more investigation and from RAN1 viewpoint we need to confirm the feasibility of RACH-less HO in NTN.
- RACH-less HO for NTN is de-prioritized in this release.
:::info
**Initial Proposal 2-3-1**

RACH-less HO for NTN is de-prioritized in this release.
:::
- In RAN2, RACH-less HO has been deprioritized.

---

#### ==Updated proposal based on company views==
:::warning
**Moderator Recommendation 2-3-1**

RAN1 should wait for RAN2 progress on RACH-less HO support in NTN
:::


## Issue#3: Indication of frequency precompensation offsets
:::success
**Agreement in RAN1#103-e**

An NR NTN UE in RRC_IDLE, RRC_INACTIVE and RRC_CONNECTED states shall be capable of at least using its acquired GNSS position and satellite ephemeris to perform frequency pre-compensation to counter shift the Doppler experienced on the service link.
:::

### Issue#3-1: Reference point for UL frequency synchronization

:::warning
**FL recommendation 3-1**

Focus the technical discussions on the features to be supported in the specs to avoid spending times on synchronization reference point definitions which is more a question of implementation.
:::
- Half of the companies agree, while the others think that the reference point location will have impacts on the implementation.


:::warning
**FL recommendation 3-1**

Focus the technical proposals on the features to be supported in the specs to avoid spending times on synchronization reference point definitions which is more a question of implementation.
:::

### Issue#3-2: Indication of frequency pre-compensation offset on DL
This issue was also discussed in RAN1#103-e.
- It is beneficial to support common frequency offset pre-compensation on DL transmissinos at gNB.
- In some NTN scenarios, it is needed to reduce UE complexity by keeping up a reasonable size for the PSS/SSS searching space.
    - A UE has to search for PSS/SSS on the sync raster.

The following options have been mentioned in the Tdocs:
:::warning
- Indication of the absolute frequency offset
    - The granularity and unit are FFS
- Indication of the reference point location w.r.t. which the Doppler DL precompensation is performed
    - This can only help deriving the part of the pre-compensated frequency offset related to Doppler.
    - The format is FSS. 
:::
Supportive [CMCC, Xiaomi, Ericsson, Qualcomm, Huawei, Thales, CATT]:
- A UE that uses the gNB DL frequency as frequency reference (which is the typical UE behaviour) needs this information to determine its nominal UL TX frequency
- The TX frequency offset at the satellite transmitter relative to the nominal DL TX frequency of the service link shall be indicated.

Not supportive [Huawei?, Intel]:
- Depending on the UE implementation [Huawei] or the pre/post compensation implementation at gNB side [Intel], the indication of this DL frequency offset may not be needed.

:::info
**Initial proposal 3-2**

If NR NTN gNB applies frequency pre-compensation in DL, the gNB should broadcast parameters giving the amount of frequency pre-compensation. These parameter should indicate the TX frequency offset at the satellite transmitter relative to the nominal DL TX frequency of the service link
- How to indicate this offset is FFS. 
:::
- Most companies support the proposal.
- The companies not supportive of the initial proposal have comments below:
    - [OPPO] This TX frequency offset is common to all the UEs.
    - [MTK] This issue needs to be further discussed in RAN1 or RAN4.
    - [vivo] The pre-compensated common frequency offset applied for DL can be the same as the post-compensated common frequency offset applied for UL.
    - [LG] It should be clarified the difference between the indication of pre-compensation frequency offset on DL and the indication of pre-compensation frequency offset on UL. To be specific, if these two pre-compensation values could be equal or similar, we don’t need to provide both parameters to NTN UE.
    
---
#### ==Updated proposal based on company views==
- [MTK] prefer to restrict sync raster so common DL frequency pre-compensation may be avoided.
    - Moderator: no incompatibility between the feature proposed here and the solution proposed by MTK.
- [LG] asked for further clarification on the difference between the indication of pre-compensation frequency offset on DL and the indication of pre-compensation frequency offset on UL.
    - Moderator: 
        - Based on Moderator understanding, the motivation for initial proposal 3-2 (i.e. indication of pre-compensation frequency offset on DL) is the following:
            - When the gNB applies a common frequency pre-compensation in DL, UEs that use the gNB DL frequency as frequency refererence (which is the typical UE behaviour) need to know the amount of frequency pre-compensated to determine its nominal UL TX frequency. Since the UE use its DL RX frequency (locked on DL reference signals) to generate its UL TX frequency, it needs to know to which amount this DL RX frequency is shifted w.r.t to the reference DL frequency of the service link. To retrieve this information, it must be aware of the common frequency shift due to gNB pre-compensation. The frequency budget details can be found in [16].
        - On the other hand, the motivation for initial proposal 3-3 is the following:
            - To enable flexible gNB implementation (e.g. no post compensation of feeder link Doppler shift), it is beneficial in some scenarios to indicate to all UEs a common frequency offset to be applied by all the UEs in addition to their self-estimated frequency pre-compensation.

:::info
**Updated proposal 3-2**

If NR NTN gNB applies frequency pre-compensation in DL, the gNB may broadcast parameters giving the amount of frequency pre-compensation. These parameter should indicate the TX frequency offset at the satellite transmitter relative to the nominal DL TX frequency of the service link.
:::

#### Updated proposal based on companies view
:::danger
Need to be updated
:::

### Issue#3-3: Indication of pre-compensation frequency offset on UL
This issue was also discussed in RAN1#103-e.
- If an NR NTN UE is capable of applying at each transmission a common frequency offset indicated by the network in addition to the geometry based frequency pre-compensation, the shift the Doppler experienced on the service link can be countered.

:::info
**Initial proposal 3-3**

Support the indication by the network of a common precompensation frequency offset on UL. 

When indicated, an NR NTN UE shall be capable to apply this offset at each transmission in addition to the UE-specific frequency pre-compensation to counter shift the Doppler experienced on the service link.
- How to indicate this UL common frequency offset is FFS
:::

- About half amount of the companies support this proposal.
- The other companies think that there is no need for indicating DL pre-compensation and UL post-compensation separately because they can be the same value.
    - Moreover, [Apple] UE can simply pre-compensate the service link Doppler.
    - [QC] The necessity of a common UL frequency compensation is unclear.


## Issue#4: Close control loop for UL frequency alignment
:::success
**Agreement in RAN1#103-e**

An NR NTN UE in RRC_IDLE, RRC_INACTIVE and RRC_CONNECTED states shall be capable of at least using its acquired GNSS position and satellite ephemeris to perform frequency pre-compensation to counter shift the Doppler experienced on the service link. This can be seen as an open control loop to maintain UL frequency synchronization.
:::
- This agreement can be seen as an open control loop to maintain UL frequency synchronization.
- [Qualcomm, Xiaomi] proposed to support closed loop frequency control commands via MAC-CE.
    - However, the benefits of such solution have not been discussed in detail.
- [Huawei, Spreadtrum Communications] explicitly mentioned that the introduction closed-loop UL frequency compensation is not needed for GNNS equipped UE.

:::warning
**FL recommendation 4**

RAN1 to further investigate the needs and benefits to support closed-loop UL frequency compensation for GNSS equipped NR NTN UE
:::
- Half-half
- [Huawei] A GNSS UE can calculate the frequency offset for its UL transmissions in RRC connected mode based on the frequency offset estimated by tracking DL reference signal and the indicated frequency pre-compensation.
    - The accuracy of estimated frequency offset for UL frequency adjustment can be ensured even without closed-loop frequency compensation.


## Issue#5: UE time/frequency synchronization based on GNSS-acquired frequency reference and time stamps

:::info
**Initial proposal 5-1-1**

Self-adjustment by the UE based on GNSS time and the time provided by referenceTimeInfo-R16 is a feasible solution and should be standardized as well.
:::
- Most companies do not support initial proposal 5-1-1.
    - whether there is spec impact or not
    - RAN1#102-e has already agreed that the time stamp method and ephemeris methods.
    - The self-adjustment by the UE can be left for implementation.
    - This is a second solution for the same issue.

:::info
**Initial proposal 5-1-2**

UE frequency adjustment based on GNSS-acquired frequency reference, DL signals and measurements and the time provided by referenceTimeInfo-R16 is a feasible solution and should be standardized as well.
:::
- Most companies do not support initial proposal 5-1-2.
    - The comments from companies are almost the same as in Initial proposal 5-1-1.


## Issue#6: Serving satellite ephemeris format
Discussions about satellite ephemeris have already started during RAN1#103e. The satellite ephemeris format to be used is still undecided. Two main options are foreseen:

:::warning
**Option 1**

Adopt a satellite ephemeris format based on ==orbital elements==. The date associated to the state vectors should be provided (implicit or explicit methods can be further discussed). Possibly, additional elements associated to the propagator model considered can be included (e.g., drag term from TLE format).
:::
:::warning
**Option 2**

Adopt a satellite ephemeris format based on ==satellite position and velocity state vectors==. The date associated to the state vectors should also be provided (implicit or explicit method can be further discussed). Possibly, additional elements associated to the propagator model considered can be included.
:::

> 老實說看不太懂上面這段。
> [color=#e85f63][name=Daniel][time=Thu, Jan 28, 2021 4:28 PM]

:::info
**Initial proposal 6-1**

NTN UE should have the capability of satellite trajectory prediction based on any provided orbit representation at a reference time.
:::
- Most companies support this proposal but also think this proposal needs clarification about the prediction capability.

:::info
**Initial proposal 6-2**

RAN1 to at least support ephemeris format based on satellite position and velocity state vectors
- Details on state vectors formats are FFS
- Details on time reference provisioning/format are FFS 
:::

- [Panasonic] Priority should be given to the ==orbital element== format due to its better efficiency, i.e., less frequent SIB updates are required compared to the PVT format.

:::warning
**FL recommendation 6-1**

RAN1 to further investigate the details regarding ephemeris formats based at least on satellite position and velocity state vectors
- Explicit or implicit time reference
- Range/Granularity/Units for position and velocity vector elements
- Separate formats for GEO orbits, LEO orbits and HAPS/ATG
:::
- Initial proposal 6-2 should be discussed first.

## Issue#7: GNSS accuracy requirement
:::success
**FL recommendation in RAN1#103-e**

RAN1 to consider the assumptions defined by RAN4 on GNSS positioning accuracy.
:::
:::success
**FL recommendation in RAN1#103-e**

It is up to RAN4 to decide whether interruptions or measurement gaps are required for GNSS measurements during NTN operation.
:::

:::info
**Initial Proposal 7-1**

It is up to RAN4 to decide whether interruptions or measurement gaps are required for GNSS measurements during NTN operation
:::
- Agreed

## Issue#8: UL Time and frequency synchronization requirements

:::info
**Initial proposal 8-1**

RAN1 should send an LS to RAN4 with the following questions:

Question 1: RAN1 would like to ask RAN4, to indicate what are the NTN UL time synchronization requirements?
- Initial access (i.e. PRACH transmission)
- UL transmissions in RRC Connected State

Question 2: RAN1 would like to ask RAN4, to indicate what are the NTN UL frequency synchronization requirements?

Question 3: RAN1 would like to ask RAN4, to indicate what are the implication of  NTN UL synchronization requirements on satellite position and velocity?
:::
- [ZTE] W.r.t the Q3, not sure whether RAN4 is cable to define the “implication” to the requirement on satellite position and velocity. From testing perspective, RAN4 only cares about the RRM performance. The requirement on the satellite position and velocity seems to be more RAN1/2 issue.

## Issue#9: UE centric precompensation

An alternative solution that would simplify the time and frequency compensation mechanisms was proposed by [Ericsson]: 

> If the position of a reference point of the feeder link and the UL and DL carrier frequencies of the feeder link are signalled to the UE, the UE can autonomously determine the time and frequency offset of both the service link and the link between the satellite and the reference point of the feeder link, which would simplify the time and frequency compensation procedures.
> [color=black]

:::info
**Initial proposal 9-1**

Support broadcasting a reference point of the feeder link and UE autonomous determination of the time and frequency offset of both the service link and the link between the satellite and the reference point of the feeder link.
:::
- Most companies do not see the need to have this proposal.
    - With the indication for the timing and frequency compensation informatino of network, UE can do synchronization with RP.
    - The RP is handled via the common TA.
- [MTK] This proposal need clarification and amendment.
- [APT] Security concern on broadcasting the location of gNB.


---



# RAN1#102-e
## Agreements
### Agreement 1
- In Rel-17 NR NTN, at least support UE which can derive based on its GNSS implementation one or more of:
    - its position 
    - a reference time and frequency
- And, based on one or more of these elements together with additional information (e.g., serving satellite ephemeris or timestamp) signalled by the network, can compute timing and frequency, and apply timing advance and frequency adjustment at least for UE in RRC idle/inactive mode.
- FFS:  Details on additional information signalled from network

### Agreement 2
In case of GNSS-assisted TA acquisition in RRC idle/inactive mode, the UE calculates its TA based on the following potential contributions:
- The User specific TA which is estimated by the UE:
    - Option 1（==用 GNSS 位置決定 user specific TA==）: The User specific TA is estimated by the UE based on its GNSS acquired position together with the serving satellite ephemeris indicated by the network:
        - FFS: Details on serving satellite ephemeris indication 
    - Option 2（==用 GNSS reference time 決定 user specific TA==）: The User specific TA  is estimated by the UE based on the GNSS acquired reference time at UE together with reference time as indicated by the network
- The Common TA if indicated by the network:
    - FFS: The need and details of Common TA indication 
- FFS: The TA margin, if needed and indicated by the network (in order to account for the TA estimation uncertainty)


# RAN1#103-e
## Email Discussion Thread


## Agreements
### Agreement 1
- An NTN UE in RRC_IDLE and RRC_INACTIVE states is required to at least support UE specific TA calculation based at least on its GNSS-acquired position and the serving satellite ephemeris.

### Agreement 2
- An NR NTN UE in RRC_IDLE and RRC_INACTIVE states shall be capable of at least using its acquired GNSS position and satellite ephemeris to calculate frequency pre-compensation to counter shift the Doppler experienced on the service link.

### Agreement 3
- In NTN, the network may broadcast
    - A common timing offset value
        - FFS details of the common timing offset
    - ==FFS: A common timing drift rate==
- Before Msg1/MsgA transmission, the NR NTN UE in idle/inactive mode calculates its TA as follows:
        $TA=(N_{TA} + N_{TA, offset}[+X]) \times T_C[+X]$
    - $N_{TA}$ is derived from the User specific TA self-estimation
    - $X$ is derived at least from the common timing offset value if broadcasted by the network. The granularity of $X$ and whether $X$ is indicated as a Timing Advance or as a Timing Offset value [unit] are FFS. Upon resolving the FFS, one of the $X$ in the equation will be removed.
    - $N_{TA, offset}$ depends on band and LTE/NR coexistence and is specified in TS 38.213 section 4.2.
    - $T_C$ is specified in TS 38.211 section 4.1
- Note: UE will not assume that the ==RTT== between UE and gNB is equal to the calculated TA for Msg1/MsgA.

### Agreement 4
- An NR NTN UE in RRC_CONNECTED states shall be capable of at least using its acquired GNSS position and satellite ephemeris to perform frequency pre-compensatino to counter shift the Doppler experienced on the service link.
