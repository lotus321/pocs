# MP4Box

## version 

    MP4Box 0.7.1

## description

```txt
GPAC version 0.7.1-rev0-g440d475-HEAD
```

## download link

    

## others

    please send email to  teamseri0us360@gmail.com if you have any questions.

---------------------

## DumpTrackInfo@filedump.c:2058-65___null-pointer-dereference

### description

    An issue was discovered in MP4Box 0.7.1, There is a/an null-pointer-dereference in function DumpTrackInfo at filedump.c:2058-65

### commandline

    MP4Box -info @@

### source

```c
2054 		esd = gf_isom_get_esd(file, trackNum, 1);
2055 		if (!esd) {
2056 			fprintf(stderr, "WARNING: Broken MPEG-4 Track\n");
2057 		} else {
>2058 			const char *st = gf_odf_stream_type_name(esd->decoderConfig->streamType);
2059 			if (st) {
2060 				fprintf(stderr, "MPEG-4 Config%s%s Stream - ObjectTypeIndication 0x%02x\n",
2061 				        full_dump ? "\n\t" : ": ", st, esd->decoderConfig->objectTypeIndication);
2062 			} else {
2063 				fprintf(stderr, "MPEG-4 Config%sStream Type 0x%02x - ObjectTypeIndication 0x%02x\n",

```

### my dbg

```c

Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x0000000000428432 in DumpTrackInfo (file=0x655010, trackID=0x2, full_dump=GF_FALSE) at filedump.c:2058
2058                            const char *st = gf_odf_stream_type_name(esd->decoderConfig->streamType);
gdb-peda$ p esd
$1 = (GF_ESD *) 0x659bb0
gdb-peda$ p esd->decoderConfig
$2 = (GF_DecoderConfig *) 0x0

```
### bug report

```txt
[33m[iso file] more than one stts entry at the end of the track with sample_delta=0 - forbidden ! Fixing to 1
[0m[31m[ODF] Error reading descriptor (tag 4 size 47): Invalid MPEG-4 Descriptor
[0m[33m[iso file] Unknown box type ....
[0m[31m[iso file] Read Box type .... (0x01000000) has size 0 but is not at root/file level, skipping
[0m[33m[iso file] Unknown box type ... 
[0m[33m[iso file] Unknown box type ...<
[0m[33m[iso file] Unknown box type ...<
[0m[33m[iso file] Unknown box type ...<
[0m[33m[iso file] Unknown box type <JC.
[0m[31m[iso file] Incomplete box UNKN
[0m[31m[iso file] Incomplete file while reading for dump - aborting parsing
[0m* Movie Info *
	Timescale 600 - 2 tracks
	Computed Duration 00:00:04.966 - Indicated Duration 00:00:04.966
	Fragmented File: no
	File suitable for progressive download (moov before mdat)
	File Brand mp42 - version 1
		Compatible brands: mp42 mp41
	Created: GMT Fri Oct 28 17:46:46 2005
	Modified: GMT Fri Oct 28 17:46:46 2005

File has no MPEG4 IOD/OD

Track # 1 Info - TrackID 1 - TimeScale 32000
Media Duration 00:00:00.004 - Indicated Duration 00:00:04.992
Track has 1 edit lists: track duration is 00:00:04.966
Media Info: Language "English (eng)" - Type "soun:mp4a" - 156 samples
MPEG-4 Config: Audio Stream - ObjectTypeIndication 0x40
MPEG-4 Audio AAC LC - 2 Channel(s) - SampleRate 32000
Self-synchronized
	RFC6381 Codec Parameters: mp4a.40.2
	All samples are sync

Track # 2 Info - TrackID 2 - TimeScale 30
Media Duration 00:00:00.000 - Indicated Duration 00:00:04.966
Track has 1 edit lists: track duration is 00:00:04.966
Media Info: Language "English (eng)" - Type "vide:mp4v" - 0 samples
Visual Track layout: x=0 y=0 width=190 height=240
AddressSanitizer:DEADLYSIGNAL
=================================================================
==4953==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000008 (pc 0x0000005794e8 bp 0x7ffde6e30af0 sp 0x7ffde6e0aba0 T0)
==4953==The signal is caused by a READ memory access.
==4953==Hint: address points to the zero page.
    #0 0x5794e7 in DumpTrackInfo /src/gpac/applications/mp4box/filedump.c:2058:65
    #1 0x587ca2 in DumpMovieInfo /src/gpac/applications/mp4box/filedump.c:2950:3
    #2 0x54d944 in mp4boxMain /src/gpac/applications/mp4box/main.c:4305:9
    #3 0x7f7c7de2f82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #4 0x4244e8 in _start (/src/gpac/installed-asan/bin/MP4Box+0x4244e8)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /src/gpac/applications/mp4box/filedump.c:2058:65 in DumpTrackInfo
==4953==ABORTING

```

### others

    from fuzz project None
    crash name None-00000322-1558597650.mp4
    Auto-generated by pyspider at 2019-05-23 16:16:43

    please send email to  teamseri0us360@gmail.com if you have any questions.

## gf_isom_get_original_format_type@drm_sample.c:540-33___null-pointer-dereference

### description

    An issue was discovered in MP4Box 0.7.1, There is a/an null-pointer-dereference in function gf_isom_get_original_format_type at drm_sample.c:540-33

### commandline

    MP4Box -info @@

### source

```c
 536 	Media_GetSampleDesc(trak->Media, sampleDescriptionIndex, &sea, NULL);
 537 	if (!sea) return GF_BAD_PARAM;
 538 
 539 	sinf = (GF_ProtectionSchemeInfoBox*)gf_list_get(sea->protections, 0);
> 540 	if (outOriginalFormat && sinf->or \*bug=>*\ iginal_format) {
 541 		*outOriginalFormat = sinf->original_format->data_format;
 542 	}
 543 	return GF_OK;
 544 }
 545 

```

### my debug

```c
Stopped reason: SIGSEGV
0x00007ffff7865711 in gf_isom_get_original_format_type (the_file=0x655010, trackNumber=0x2, sampleDescriptionIndex=0x1, outOriginalFormat=0x7ffffffcbf94)
    at isomedia/drm_sample.c:540
540             if (outOriginalFormat && sinf->original_format) {
$1 = (GF_ProtectionSchemeInfoBox *) 0x0
```

### bug report

```txt
AddressSanitizer:DEADLYSIGNAL
=================================================================
==4969==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000020 (pc 0x7fd4b273ace9 bp 0x7ffc1d1f1070 sp 0x7ffc1d1f0fc0 T0)
==4969==The signal is caused by a READ memory access.
==4969==Hint: address points to the zero page.
    #0 0x7fd4b273ace8 in gf_isom_get_original_format_type /src/gpac/src/isomedia/drm_sample.c:540:33
    #1 0x7fd4b2961ecc in gf_media_get_rfc_6381_codec_name /src/gpac/src/media_tools/dash_segmenter.c:429:8
    #2 0x580975 in DumpTrackInfo /src/gpac/applications/mp4box/filedump.c:2647:3
    #3 0x587ca2 in DumpMovieInfo /src/gpac/applications/mp4box/filedump.c:2950:3
    #4 0x54d944 in mp4boxMain /src/gpac/applications/mp4box/main.c:4305:9
    #5 0x7fd4b112682f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #6 0x4244e8 in _start (/src/gpac/installed-asan/bin/MP4Box+0x4244e8)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /src/gpac/src/isomedia/drm_sample.c:540:33 in gf_isom_get_original_format_type
==4969==ABORTING

```

### others

    from fuzz project None
    crash name None-00000995-1558597651.mp4
    Auto-generated by pyspider at 2019-05-23 16:16:43

    please send email to  teamseri0us360@gmail.com if you have any questions.

## GetESD@track.c:271-28___null-pointer-dereference

### description

    An issue was discovered in MP4Box 0.7.1, There is a/an null-pointer-dereference in function GetESD at track.c:271-28

### commandline

    MP4Box -info @@

### source

```c
 267 		if (
 268 #ifndef GPAC_DISABLE_ISOM_FRAGMENTS
 269 		    moov->mvex &&
 270 #endif
> 271 		    (esd->decoderConfig->streamType==GF_STREAM_VISUAL)) {
 272 			esd->slConfig->hasRandomAccessUnitsOnlyFlag = 0;
 273 			esd->slConfig->useRandomAccessPointFlag = 1;
 274 			if (trak->moov->mov->openMode!=GF_ISOM_OPEN_READ)
 275 				stbl->SyncSample = (GF_SyncSampleBox *) gf_isom_box_new(GF_ISOM_BOX_TYPE_STSS);
 276 		} else {

```

### mydebug

```c
Stopped reason: SIGSEGV
0x00007ffff78a6829 in GetESD (moov=0x656790, trackID=0x2, StreamDescIndex=0x1, outESD=0x7ffffffcbfe0) at isomedia/track.c:271
271                         (esd->decoderConfig->streamType==GF_STREAM_VISUAL)) {
gdb-peda$ p esd->decoderConfig
$1 = (GF_DecoderConfig *) 0x0
```

### bug report

```txt
[33m[iso file] Unknown box type schh
[0m[33mICC colour profile not supported 
[0m[33m[iso file] Unknown box type ....
[0m[31m[iso file] Incomplete box UNKN
[0m[31m[iso file] Incomplete file while reading for dump - aborting parsing
[0m* Movie Info *
	Timescale 90000 - 2 tracks
	Computed Duration 00:00:00.000 - Indicated Duration 00:00:00.000
	Fragmented File: yes - duration 00:00:05.589
1 fragments - 1 SegmentIndexes
	File suitable for progressive download (moov before mdat)
	File Brand iso5 - version 1
		Compatible brands: avc1 .so5 dash
	Created: GMT Wed Mar 26 00:20:53 2014
	Modified: GMT Wed Mar 26 00:20:54 2014

File has root IOD (9 bytes)
Scene PL 0xff - Graphics PL 0xff - OD PL 0xff
Visual PL: AVC/H264 Profile (0x7f)
Audio PL: High Quality Audio Profile @ Level 2 (0x0f)
No streams included in root OD

iTunes Info:
	Encoder Software: HandBrake 0.9.9 2013052900
1 UDTA types: meta (1) 

Track # 1 Info - TrackID 1 - TimeScale 90000
Media Duration 00:00:00.000 - Indicated Duration 00:00:00.000
Media Info: Language "Undetermined (und)" - Type "vide:encv" - 0 samples
Fragmented track: 15 samples - Media Duration 00:00:00.500
Visual Track layout: x=0 y=0 width=560 height=320
MPEG-4 Config: Visual Stream - ObjectTypeIndication 0x21
AVC/H264 Video - Visual Size 560 x 320
	AVC Info: 0 SPS - 0 PPS - Profile Baseline @ Level 3
	NAL Unit length bits: 32
	Chroma format YUV 4:2:0 - Luma bit depth 8 - chroma bit depth 8
Self-synchronized

*Encrypted stream - unknown scheme 
[33m[ISOM Tools] Unkown protection scheme type 
[0m	RFC6381 Codec Parameters: avc1.42C01E
	All samples are sync

Track # 2 Info - TrackID 2 - TimeScale 48000
Media Duration 00:00:00.000 - Indicated Duration 00:00:00.000
Media Info: Language "English (eng)" - Type "soun:enca" - 0 samples
Fragmented track: 24 samples - Media Duration 00:00:00.512
1 UDTA types: name (1) 
AddressSanitizer:DEADLYSIGNAL
=================================================================
==4998==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000008 (pc 0x7f500d1be119 bp 0x7ffeb6a2b510 sp 0x7ffeb6a2b2c0 T0)
==4998==The signal is caused by a READ memory access.
==4998==Hint: address points to the zero page.
    #0 0x7f500d1be118 in GetESD /src/gpac/src/isomedia/track.c:271:28
    #1 0x7f500d0e230a in gf_isom_get_esd /src/gpac/src/isomedia/isom_read.c:1086:6
    #2 0x57946a in DumpTrackInfo /src/gpac/applications/mp4box/filedump.c:2054:9
    #3 0x587ca2 in DumpMovieInfo /src/gpac/applications/mp4box/filedump.c:2950:3
    #4 0x54d944 in mp4boxMain /src/gpac/applications/mp4box/main.c:4305:9
    #5 0x7f500baaa82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #6 0x4244e8 in _start (/src/gpac/installed-asan/bin/MP4Box+0x4244e8)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /src/gpac/src/isomedia/track.c:271:28 in GetESD
==4998==ABORTING

```

### others

    from fuzz project None
    crash name None-00001079-1558597651.mp4
    Auto-generated by pyspider at 2019-05-23 16:16:45

    please send email to  teamseri0us360@gmail.com if you have any questions.

## ReadGF_IPMPX_RemoveToolNotificationListener@ipmpx_code.c:1103-54___heap-buffer-overflow

### description

    An issue was discovered in MP4Box 0.7.1, There is a/an heap-buffer-overflow in function ReadGF_IPMPX_RemoveToolNotificationListener at ipmpx_code.c:1103-54

### commandline

    MP4Box -info @@

### source

```c
1099 {
1100 	u32 i;
1101 	GF_IPMPX_RemoveToolNotificationListener*p = (GF_IPMPX_RemoveToolNotificationListener*)_p;
1102 	p->eventTypeCount = gf_bs_read_int(bs, 8);
>1103 	for (i=0; i<p->eventTypeCount; i++) p->eventType[i] = gf_bs_read_int(bs, 8);
1104 	return GF_OK;
1105 }
1106 static u32 SizeGF_IPMPX_RemoveToolNotificationListener(GF_IPMPX_Data *_p)
1107 {
1108 	GF_IPMPX_RemoveToolNotificationListener*p = (GF_IPMPX_RemoveToolNotificationListener*)_p;

```

### bug report

```txt
[33m[iso file] Unknown box type ....
[0m=================================================================
==5055==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x60400000023c at pc 0x7fd7187bb791 bp 0x7ffc7c0c1d10 sp 0x7ffc7c0c1d08
WRITE of size 4 at 0x60400000023c thread T0
    #0 0x7fd7187bb790 in ReadGF_IPMPX_RemoveToolNotificationListener /src/gpac/src/odf/ipmpx_code.c:1103:54
    #1 0x7fd7187bb790 in GF_IPMPX_ReadData /src/gpac/src/odf/ipmpx_code.c:1981
    #2 0x7fd7187b42a3 in gf_ipmpx_data_parse /src/gpac/src/odf/ipmpx_code.c:293:6
    #3 0x7fd7187bae47 in ReadGF_IPMPX_MutualAuthentication /src/gpac/src/odf/ipmpx_code.c:440:7
    #4 0x7fd7187bae47 in GF_IPMPX_ReadData /src/gpac/src/odf/ipmpx_code.c:1985
    #5 0x7fd7187b42a3 in gf_ipmpx_data_parse /src/gpac/src/odf/ipmpx_code.c:293:6
    #6 0x7fd718790176 in gf_odf_read_ipmp /src/gpac/src/odf/odf_code.c:2421:8
    #7 0x7fd71876cf30 in gf_odf_read_descriptor /src/gpac/src/odf/desc_private.c:310:10
    #8 0x7fd71876fe7f in gf_odf_parse_descriptor /src/gpac/src/odf/descriptors.c:161:8
    #9 0x7fd71879bb29 in gf_odf_desc_read /src/gpac/src/odf/odf_codec.c:302:6
    #10 0x7fd71856a82a in esds_Read /src/gpac/src/isomedia/box_code_base.c:1259:7
    #11 0x7fd71861bc3c in gf_isom_box_read /src/gpac/src/isomedia/box_funcs.c:1323:9
    #12 0x7fd71861bc3c in gf_isom_box_parse_ex /src/gpac/src/isomedia/box_funcs.c:196
    #13 0x7fd71857f524 in audio_sample_entry_Read /src/gpac/src/isomedia/box_code_base.c:3952:8
    #14 0x7fd71861bc3c in gf_isom_box_read /src/gpac/src/isomedia/box_funcs.c:1323:9
    #15 0x7fd71861bc3c in gf_isom_box_parse_ex /src/gpac/src/isomedia/box_funcs.c:196
    #16 0x7fd71861dcdf in gf_isom_box_array_read_ex /src/gpac/src/isomedia/box_funcs.c:1217:7
    #17 0x7fd71858fdd3 in stsd_Read /src/gpac/src/isomedia/box_code_base.c:5604:9
    #18 0x7fd71861bc3c in gf_isom_box_read /src/gpac/src/isomedia/box_funcs.c:1323:9
    #19 0x7fd71861bc3c in gf_isom_box_parse_ex /src/gpac/src/isomedia/box_funcs.c:196
    #20 0x7fd71861ab97 in gf_isom_parse_root_box /src/gpac/src/isomedia/box_funcs.c:42:8
    #21 0x7fd71864182a in gf_isom_parse_movie_boxes /src/gpac/src/isomedia/isom_intern.c:204:7
    #22 0x7fd718645e05 in gf_isom_open_file /src/gpac/src/isomedia/isom_intern.c:604:19
    #23 0x7fd71864e478 in gf_isom_open /src/gpac/src/isomedia/isom_read.c:412:11
    #24 0x54acf5 in mp4boxMain /src/gpac/applications/mp4box/main.c:4106:11
    #25 0x7fd71701c82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #26 0x4244e8 in _start (/src/gpac/installed-asan/bin/MP4Box+0x4244e8)

0x60400000023c is located 0 bytes to the right of 44-byte region [0x604000000210,0x60400000023c)
allocated by thread T0 here:
    #0 0x4e8718 in malloc /work/llvm/projects/compiler-rt/lib/asan/asan_malloc_linux.cc:88
    #1 0x7fd7187b4c06 in NewGF_IPMPX_RemoveToolNotificationListener /src/gpac/src/odf/ipmpx_code.c:1091:2
    #2 0x7fd7187b4c06 in gf_ipmpx_data_new /src/gpac/src/odf/ipmpx_code.c:1714

SUMMARY: AddressSanitizer: heap-buffer-overflow /src/gpac/src/odf/ipmpx_code.c:1103:54 in ReadGF_IPMPX_RemoveToolNotificationListener
Shadow bytes around the buggy address:
  0x0c087fff7ff0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c087fff8000: fa fa 00 00 00 00 00 05 fa fa fd fd fd fd fd fd
  0x0c087fff8010: fa fa 00 00 00 00 00 00 fa fa 00 00 00 00 00 00
  0x0c087fff8020: fa fa 00 00 00 00 00 fa fa fa fd fd fd fd fd fd
  0x0c087fff8030: fa fa 00 00 00 00 00 00 fa fa 00 00 00 00 00 00
=>0x0c087fff8040: fa fa 00 00 00 00 00[04]fa fa fa fa fa fa fa fa
  0x0c087fff8050: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff8060: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff8070: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff8080: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff8090: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07 
  Heap left redzone:       fa
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
  Left alloca redzone:     ca
  Right alloca redzone:    cb
==5055==ABORTING

```

### others

    from fuzz project None
    crash name None-00001119-1558597651.mp4
    Auto-generated by pyspider at 2019-05-23 16:16:46

    please send email to  teamseri0us360@gmail.com if you have any questions.