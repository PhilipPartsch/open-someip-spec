
:orphan:

.. This file is part of the Open SOME/IP Specification.
.. 
.. Copyright (c) 2025 Technica Engineering GmbH
.. 
.. This file is licensed under the SOME/IP Community Specification License.
.. You can find a copy of this license in the Open SOME/IP Specification repository.
..
.. see https://some-ip.com and https://github.com/some-ip-com/open-someip-spec/

.. #####################################################
.. SOME/IP Transport Protocol (SOME/IP-TP) Specification
.. #####################################################


.. heading:: Transporting large SOME/IP messages of UDP (SOME/IP-TP)
    :id: feat_req_someip_759
    :layout: focus
    :style: clean

.. rst-class:: break_before

Transporting large SOME/IP messages of UDP (SOME/IP-TP)
####################################################### 

.. feat_req:: 🎯
    :id: feat_req_someip_760
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The UDP binding of SOME/IP can only transport SOME/IP messages that fit directly into a IP packet. If larger SOME/IP messages need to be transported over UDP (e.g. of 32 KB) the SOME/IP Transport Protocol (SOME/IP-TP) shall be used.
    
.. feat_req:: 🎯
    :id: feat_req_someip_764
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The SOME/IP message too big to be transported directly with the UDP binding shall be called "original" SOME/IP message. The "pieces" of the original SOME/IP message payload transported in SOME/IP-TP messages shall be called "segments".
    
.. feat_req:: 🎯
    :id: feat_req_someip_762
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
SOME/IP messages using SOME/IP-TP shall activate Session Handling (Session ID must be unique for the original message).
    
.. feat_req:: 🎯
    :id: feat_req_someip_763
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
All SOME/IP-TP segments shall carry the Session ID of the original message; thus, they have all the same Session-ID.
    
.. feat_req:: 🎯
    :id: feat_req_someip_765
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
SOME/IP-TP segments shall have the TP-Flag of the Message Type set to 1 :need:`feat_req_someip_761`.
    
.. feat_req:: 🎯
    :id: feat_req_someip_766
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True

.. rst-class:: compact
  
SOME/IP-TP segments shall have a TP header right after the SOME/IP header (i.e. before the SOME/IP payload) with the following structure (bits from highest to lowest):

* Offset [28 bits]
* Reserved Flag [1 bit]
* Reserved Flag [1 bit]
* Reserved Flag [1 bit]
* More Segments Flag [1 bit]
    
.. feat_req:: 🎯
    :id: feat_req_someip_768
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The Offset field shall transport the upper 28 bits of an uint32. The lower 4 bits shall be always be interpreted as 0.

Note: This means that the offset field can only transport offset values that are multiples of 16 bytes.
    
.. feat_req:: 🎯
    :id: feat_req_someip_767
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The Offset field of the TP header shall be set to the offset in bytes of the transported segment in the original message.
    
.. feat_req:: 🎯
    :id: feat_req_someip_769
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The Reserved Flags shall be set to 0 by the sender and shall be ignored (and not checked) by the receiver.
    
.. feat_req:: 🎯
    :id: feat_req_someip_770
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The More Segments Flag shall be set to 1 for all segments but the last segment. For the last segment it shall be set to 0.
    
.. feat_req:: 🎯
    :id: feat_req_someip_771
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The SOME/IP length field shall be used as specified before. This means it covers but the first 8 bytes of the SOME/IP header and all bytes after that.

Note: This means that for a SOME/IP-TP message transporting a segment, the SOME/IP length covers 8 bytes of the SOME/IP header, the 4 bytes of the TP header, and the segment itself.
    
.. feat_req:: 🎯
    :id: feat_req_someip_772
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The length of a segment must reflect the alignment of the next segment based on the offset field. Therefore, all but the last segment shall have a length that is a multiple of 16 bytes.
    
.. feat_req:: 🎯
    :id: feat_req_someip_773
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
Since UDP-based SOME/IP messages are limited to 1400 bytes payload, the maximum length of a segment that is correctly aligned is 1392 bytes.
    
.. feat_req:: 🎯
    :id: feat_req_someip_774
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
SOME/IP-TP messages shall use the same Message ID (i.e. Service ID and Method ID), Request ID (i.e. Client ID and Session ID), Protocol Version, Interface Version, and Return Code as the original message.

Note: As described above the Length, Message Type, and Payload are adapted by SOME/IP-TP.
    
.. feat_req:: 🎯
    :id: feat_req_someip_801
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
ECUs using SOME/IP-TP shall implement traffic shaping for the segments in order to control network bursts. The configuration of such shapers shall be discussed with the OEM.

Note: This does not mean that one shaper per SOME/IP-TP message is required but a shaper can cover multiple SOME/IP messages.
    
.. heading:: Sender specific behavior
    :id: feat_req_someip_775
    :layout: focus
    :style: clean

Sender specific behavior
************************ 

.. feat_req:: 🎯
    :id: feat_req_someip_788
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The sender shall segment only messages that were configured to be segmented.
    
.. feat_req:: 🎯
    :id: feat_req_someip_777
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The sender shall send segments in ascending order.
    
.. feat_req:: 🎯
    :id: feat_req_someip_778
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The sender shall segment in a way that all segments with the More Segment Flag set to 1 are of the same size.
    
.. feat_req:: 🎯
    :id: feat_req_someip_779
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The sender shall try to maximize the size of segments within limitations imposed by this specification.
    
.. feat_req:: 🎯
    :id: feat_req_someip_780
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The sender shall not send overlapping or duplicated segments.
    
.. feat_req:: 🎯
    :id: feat_req_someip_786
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The sender shall only use as many different Client-IDs for a Message that uses SOME/IP-TP as determined by the configuration.
    
.. heading:: Receiver specific behavior
    :id: feat_req_someip_776
    :layout: focus
    :style: clean

Receiver specific behavior
************************** 

.. feat_req:: 🎯
    :id: feat_req_someip_781
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The receiver shall match segments for reassembly based on the configured values of Message-ID, Protocol-Version, Interface-Version, and Message-Type (w/o TP Flag) as well as the dynamic value of the Request-ID.
    
.. feat_req:: 🎯
    :id: feat_req_someip_794
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The configured header values shall be used to select the "reassembly buffer" or similar.
    
.. feat_req:: 🎯
    :id: feat_req_someip_787
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
It shall be supported to reassemble the same message from different clients (difference in Sender IP, Sender Port, or Client ID but otherwise equal) in parallel. This should be controlled by configuration and determines the amount of "reassembly buffers".
    
.. feat_req:: 🎯
    :id: feat_req_someip_795
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The Session ID shall be used to detect the next original message to be reassembled. 
    
.. feat_req:: 🎯
    :id: feat_req_someip_793
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The receiver shall start a new reassembly (and possible throw away old segments that were not successfully reassembled), if a new segment with a different Session-ID is received.
    
.. feat_req:: 🎯
    :id: feat_req_someip_782
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The receiver should only reassemble up to its configured buffer size and skip the rest of the message.
    
.. feat_req:: 🎯
    :id: feat_req_someip_783
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
Only correctly reassembled message of up to the configured size shall be passed to an application.

Note: This means that the implementation must make sure that all bytes of the message must be bytes that were received and reassembled correctly. Counting non-overlapping, non-duplicated bytes and comparing this to the length could be a valid check.
    
.. feat_req:: 🎯
    :id: feat_req_someip_784
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The Return Code of the last segment used for reassembly shall be used for the reassembled message.
    
.. feat_req:: 🎯
    :id: feat_req_someip_785
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
If a Message Type of an incoming SOME/IP message is passed to the application, the TP Flag shall be reset to 0.
    
.. feat_req:: 🎯
    :id: feat_req_someip_789
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The receiver shall support reassembly of segments that are received in ascending and descending order.
    
.. feat_req:: 🎯
    :id: feat_req_someip_790
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The receiver should use limited resources to support reassembly of reordered segments of a single original message. At least a reorder distance of 3 shall be supported (i.e. segments are allowed up to 3 positions away from their correct place in sequence).

Note: This could mean for example that the receiver can only desegment if segments are in order but it stores the last 4 segments and sorts them before trying to deserialize. Or it could mean that all segments are written into a buffer and 4 meta data structures (e.g. start and length) to store which data of the buffer is valid are present.
    
.. feat_req:: 🎯
    :id: feat_req_someip_796
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The receiver shall cancel desegmentation for a single original message, if missing segments of this original message are detected and resources do not permit to further wait on the segment (e.g. because the next message already starts or reorder distance is higher than expected).

Note: This means that reordering inside a single original message is allowed, if resources permit this.
    
.. feat_req:: 🎯
    :id: feat_req_someip_802
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
Reordering between of segments that are of different original messages but using the same buffer (e.g. only the Session-ID and payload are different) is currently not allowed, since this could lead to reordering of original messages and breaking "last is best" semantics.

Note: This prohibits that equal events (same Message-ID, IP-Addresses, ports numbers, transport protocol) arrive in the wrong order, when some of their segments get reordered.
    
.. feat_req:: 🎯
    :id: feat_req_someip_803
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
Reordering of segments of completely different original messages (e.g. Message ID is different) is not of concern since those segments go to different buffers.
    
.. feat_req:: 🎯
    :id: feat_req_someip_797
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The receiver shall correctly reassemble overlapping and duplicated segments by overwriting based on the last received segment.
    
.. feat_req:: 🎯
    :id: feat_req_someip_810
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The receiver may cancel reassembly, if overlapping or duplicated segments change already written bytes in the buffer, if this feature can be turned off by configuration.
    
.. feat_req:: 🎯
    :id: feat_req_someip_792
    :reqtype: Requirement
    :security: NO
    :safety: QM
    :satisfies: 
    :status: valid
    :collapse: True
  
The receiver shall be able to detect and handle obvious errors gracefully.

E.g. cancel reassembly if segment length of a segment with MS=1 is not a multiple of 16.

Note: This means that buffer overflows or other malfunction shall must be prevented by the receiving code.
    