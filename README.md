# Layering-dump
dump layering
#============= LNBTS ===============#
select
a.mrbtsId,
a.lnBtsId,
sales_area,
mc_cluster,
nano_cluster,
actIdleLB,
actMeasBasedIMLB,
@actIdleLB:=1 AS thd_actIdleLB,
@actMeasBasedIMLB:=0 AS thd_actMeasBasedIMLB,
CONCAT("PLMN-PLMN/MRBTS-",a.mrbtsId,"/LNBTS-",a.lnBtsId) as DN
FROM a_lte_mrbts_lnbts a
INNER JOIN 4g_list_mocn_ns b on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId
WHERE sales_area = 'Medan'
GROUP BY a.mrbtsId,
a.lnBtsId;


#============= lncell ===============#

SELECT
a.mrbtsId,
a.lnBtsId,
a.lncelid,
a.earfcnDL,
@band := CASE WHEN a.earfcnDL IN (3652,3676) THEN 'L900'
		 WHEN a.earfcnDL IN (1475,1625) THEN 'L1800'
		 WHEN a.earfcnDL IN (75,500,525,100,200) THEN 'L2100'
		 ELSE NULL END AS BAND,
a.sales_area,
a.mc_cluster,
a.nano_cluster,
idleLBCelResWeight,
idleLBCellReSelPrio,
idleLBPercentageOfUes,
mlbEicicOperMode,
actIntraFreqLoadBal,
actInterFreqLB,
actAmle,
nomNumPrbNonGbr,
targetLoadGbrDl,
targetLoadNonGbrDl,
targetLoadPdcch,
highLoadGbrDl,
highLoadNonGbrDl,
highLoadPdcch,
iFLBHighLoadGBRDL,
iFLBHighLoadNonGBRDL,
iFLBHighLoadPdcch,
iFLBRetryTimer,
iFLBBearCheckTimer,
iFLBA4ActLim,
amleMaxNumHo,
amlePeriodLoadExchange,
cellCapClass,
t320,
enableBetterCellHo,
enableCovHo,
threshold1,
a3TriggerQuantityHO,
a3Offset,
a3TimeToTrigger,
a3ReportInterval,
hysA3Offset,
threshold3,
hysThreshold3,
threshold3a,
threshold2a,
hysThreshold2a,
threshold2InterFreq,
hysThreshold2InterFreq,
threshold2Wcdma,
hysThreshold2Wcdma,
threshold2GERAN,
hysThreshold2GERAN,
threshold4,
hysThreshold4,
drxProfile101drxInactivityT,
drxProfile103drxInactivityT,
idleLBCapThresh,
NULL AS thd_idleLBCelResWeight,
NULL AS thd_idleLBCellReSelPrio,
@idleLBPercentageOfUes:= 100 AS thd_idleLBPercentageOfUes,
@mlbEicicOperMode:=2 AS thd_mlbEicicOperMode,
@actIntraFreqLoadBal:=0 AS thd_actIntraFreqLoadBal,
@actInterFreqLB:= IF(@band = "L900",1,0) AS thd_actInterFreqLB,
@actAmle:=1 AS thd_actAmle,
@nomNumPrbNonGbr:= IF(a.earfcnDL = 75 or a.earfcnDL = 1625,10,IF(a.earfcnDL = 200 or a.earfcnDL = 1475,1,50)) AS thd_nomNumPrbNonGbr,
@targetLoadGbrDl:=75 AS thd_targetLoadGbrDl,
@targetLoadNonGbrDl:=75 AS thd_targetLoadNonGbrDl,
@targetLoadPdcch:=75 AS thd_targetLoadPdcch,
@highLoadGbrDl:=80 AS thd_highLoadGbrDl,
@highLoadNonGbrDl:=80 AS thd_highLoadNonGbrDl,
@highLoadPdcch:=90 AS thd_highLoadPdcch,
@iFLBHighLoadGBRDL:=85 AS thd_iFLBHighLoadGBRDL,
@iFLBHighLoadNonGBRDL:=85 AS thd_iFLBHighLoadNonGBRDL,
@iFLBHighLoadPdcch:=85 AS thd_iFLBHighLoadPdcch,
@iFLBRetryTimer:=7 AS thd_iFLBRetryTimer,
@iFLBBearCheckTimer:=5 AS thd_iFLBBearCheckTimer,
@iFLBA4ActLim:= IF(a.earfcnDL = 75 or a.earfcnDL = 1625,50,IF(a.earfcnDL = 200 or a.earfcnDL = 1475,5,50)) AS thd_iFLBA4ActLim,
@amleMaxNumHo:=100 AS thd_amleMaxNumHo,
@amlePeriodLoadExchange:=1000 AS thd_amlePeriodLoadExchange,
@cellCapClass:= IF(a.earfcnDL = 75 or a.earfcnDL = 1625,100,IF(a.earfcnDL = 200 or a.earfcnDL = 1475,50,50)) AS thd_cellCapClass,
@t320:=5 AS thd_t320,
@enableBetterCellHo:=1 AS thd_enableBetterCellHo,
@enableCovHo:=1 AS thd_enableCovHo,
@threshold1:= IF(@band = "L900",97,90) AS thd_threshold1,
@a3TriggerQuantityHO:=1 AS thd_a3TriggerQuantityHO,
@a3Offset:=6 AS thd_a3Offset,
@a3TimeToTrigger:=8 AS thd_a3TimeToTrigger,
@a3ReportInterval:=3 AS thd_a3ReportInterval,
@hysA3Offset:=2 AS thd_hysA3Offset,
@threshold3:=30 AS thd_threshold3,
@hysThreshold3:=0 AS thd_hysThreshold3,
@threshold3a:=32 AS thd_threshold3a,
@threshold2a:= IF(a.earfcnDL = 75 or a.earfcnDL = 1625,29,IF(a.earfcnDL = 200 or a.earfcnDL = 1475,34,96)) AS thd_threshold2a,
@hysThreshold2a:=2 AS thd_hysThreshold2a,
@hysThreshold2a:= IF(a.earfcnDL = 75 or a.earfcnDL = 1625,27,IF(a.earfcnDL = 200 or a.earfcnDL = 1475,32,95)) AS thd_threshold2InterFreq,
@hysThreshold2InterFreq:=2 AS thd_hysThreshold2InterFreq,
@threshold2Wcdma:=0 AS thd_threshold2Wcdma,
@hysThreshold2Wcdma:=0 AS thd_hysThreshold2Wcdma,
@threshold2GERAN:=0 AS thd_threshold2GERAN,
@hysThreshold2GERAN:=0 AS thd_hysThreshold2GERAN,
@threshold4:=0 AS thd_threshold4,
@hysThreshold4:=0 AS thd_hysThreshold4,
@drxProfile101drxInactivityT:=10 AS thd_drxProfile101drxInactivityT,
@drxProfile103drxInactivityT:=10 AS thd_drxProfile103drxInactivityT,
@idleLBCapThresh:=100 AS thd_idleLBCapThresh,
CONCAT("PLMN-PLMN/MRBTS-",a.mrbtsId,"/LNBTS-",a.lnBtsId,"/LNCEL-",a.lncelid) as DN
FROM
(SELECT
a.mrbtsId,
a.lnBtsId,
a.lncelid,
a.earfcnDL,
a.sales_area,
a.mc_cluster,
a.nano_cluster,
idleLBCelResWeight,
idleLBCellReSelPrio,
idleLBPercentageOfUes,
mlbEicicOperMode,
actIntraFreqLoadBal,
actInterFreqLB,
actAmle,
nomNumPrbNonGbr,
targetLoadGbrDl,
targetLoadNonGbrDl,
targetLoadPdcch,
highLoadGbrDl,
highLoadNonGbrDl,
highLoadPdcch,
iFLBHighLoadGBRDL,
iFLBHighLoadNonGBRDL,
iFLBHighLoadPdcch,
iFLBRetryTimer,
iFLBBearCheckTimer,
iFLBA4ActLim,
amleMaxNumHo,
amlePeriodLoadExchange,
cellCapClass,
t320,
enableBetterCellHo,
enableCovHo,
threshold1,
a3TriggerQuantityHO,
a3Offset,
a3TimeToTrigger,
a3ReportInterval,
hysA3Offset,
threshold3,
hysThreshold3,
threshold3a,
threshold2a,
hysThreshold2a,
threshold2InterFreq,
hysThreshold2InterFreq,
threshold2Wcdma,
hysThreshold2Wcdma,
threshold2GERAN,
hysThreshold2GERAN,
threshold4,
hysThreshold4

FROM

(SELECT
a.mrbtsId,
a.lnBtsId,
a.lncelid,
earfcnDL,
sales_area,
mc_cluster,
nano_cluster,
idleLBCelResWeight,
idleLBCellReSelPrio,
idleLBPercentageOfUes,
mlbEicicOperMode,
actIntraFreqLoadBal,
actInterFreqLB,
actAmle,
nomNumPrbNonGbr,
targetLoadGbrDl,
targetLoadNonGbrDl,
targetLoadPdcch,
highLoadGbrDl,
highLoadNonGbrDl,
highLoadPdcch,
iFLBHighLoadGBRDL,
iFLBHighLoadNonGBRDL,
iFLBHighLoadPdcch,
iFLBRetryTimer,
iFLBBearCheckTimer,
iFLBA4ActLim,
amleMaxNumHo,
amlePeriodLoadExchange,
cellCapClass,
t320
FROM
(select
a.mrbtsId,
a.lnBtsId,
a.lncelid,
sales_area,
mc_cluster,
nano_cluster,
idleLBCelResWeight,
idleLBCellReSelPrio,
idleLBPercentageOfUes,
mlbEicicOperMode,
actIntraFreqLoadBal,
actInterFreqLB,
actAmle,
nomNumPrbNonGbr,
targetLoadGbrDl,
targetLoadNonGbrDl,
targetLoadPdcch,
highLoadGbrDl,
highLoadNonGbrDl,
highLoadPdcch,
iFLBHighLoadGBRDL,
iFLBHighLoadNonGBRDL,
iFLBHighLoadPdcch,
iFLBRetryTimer,
iFLBBearCheckTimer,
iFLBA4ActLim,
amleMaxNumHo,
amlePeriodLoadExchange,
cellCapClass,
t320
FROM a_lte_lncel_lc a
INNER JOIN 4g_list_mocn_ns b on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId and a.lncelid = b.lncelid) a
INNER JOIN a_lte_mrbts_lnbts_lncel_lncel_fdd b on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId and a.lncelid = b.lncelid
WHERE sales_area = 'Medan' and earfcnDL not in (500,525,100,125,3676,476,76,474)) a

INNER JOIN

(SELECT
a.mrbtsId,
a.lnBtsId,
a.lncelid,
earfcnDL,
sales_area,
mc_cluster,
nano_cluster,
enableBetterCellHo,
enableCovHo,
threshold1,
a3TriggerQuantityHO,
a3Offset,
a3TimeToTrigger,
a3ReportInterval,
hysA3Offset,
threshold3,
hysThreshold3,
threshold3a,
threshold2a,
hysThreshold2a,
threshold2InterFreq,
hysThreshold2InterFreq,
threshold2Wcdma,
hysThreshold2Wcdma,
threshold2GERAN,
hysThreshold2GERAN,
threshold4,
hysThreshold4
FROM

(SELECT
a.mrbtsId,
a.lnBtsId,
a.lncelid,
sales_area,
mc_cluster,
nano_cluster,
enableBetterCellHo,
enableCovHo,
threshold1,
a3TriggerQuantityHO,
a3Offset,
a3TimeToTrigger,
a3ReportInterval,
hysA3Offset,
threshold3,
hysThreshold3,
threshold3a,
threshold2a,
hysThreshold2a,
threshold2InterFreq,
hysThreshold2InterFreq,
threshold2Wcdma,
hysThreshold2Wcdma,
threshold2GERAN,
hysThreshold2GERAN,
threshold4,
hysThreshold4
FROM a_lte_lncel_hc a
INNER JOIN 4g_list_mocn_ns b on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId and a.lncelid = b.lncelid) a
INNER JOIN a_lte_mrbts_lnbts_lncel_lncel_fdd b on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId and a.lncelid = b.lncelid
WHERE sales_area = 'Medan' and earfcnDL not in (500,525,100,125,3676,476,76,474)) b 
on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId and a.lncelid = b.lncelid) a

INNER JOIN

(SELECT
a.mrbtsId,
a.lnBtsId,
a.lncelid,
a.earfcnDL,
a.sales_area,
a.mc_cluster,
a.nano_cluster,
drxProfile101drxInactivityT,
drxProfile103drxInactivityT,
idleLBCapThresh
FROM
(SELECT
a.mrbtsId,
a.lnBtsId,
a.lncelid,
earfcnDL,
sales_area,
mc_cluster,
nano_cluster,
drxProfile101drxInactivityT,
drxProfile103drxInactivityT
FROM
(select
a.mrbtsId,
a.lnBtsId,
a.lncelid,
sales_area,
mc_cluster,
nano_cluster,
drxProfile101drxInactivityT,
drxProfile103drxInactivityT
FROM a_lte_lncel_dc a
INNER JOIN 4g_list_mocn_ns b on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId and a.lncelid = b.lncelid) a
INNER JOIN a_lte_mrbts_lnbts_lncel_lncel_fdd b on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId and a.lncelid = b.lncelid
WHERE sales_area = 'Medan' and earfcnDL not in (500,525,100,125,3676,476,76,474)) a

INNER JOIN

(SELECT
a.mrbtsId,
a.lnBtsId,
a.lncelid,
earfcnDL,
sales_area,
mc_cluster,
nano_cluster,
idleLBCapThresh
FROM
(SELECT
a.mrbtsId,
a.lnBtsId,
a.lncelid,
sales_area,
mc_cluster,
nano_cluster,
idleLBCapThresh
FROM a_lte_mrbts_lnbts_lncel a
INNER JOIN 4g_list_mocn_ns b on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId and a.lncelid = b.lncelid) a
INNER JOIN a_lte_mrbts_lnbts_lncel_lncel_fdd b on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId and a.lncelid = b.lncelid
WHERE sales_area = 'Medan' and earfcnDL not in (500,525,100,125,3676,476,76,474)) b
on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId and a.lncelid = b.lncelid) b 
on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId and a.lncelid = b.lncelid;

#====== IRFIM ========#
SELECT
a.mrbtsId,
a.lnbtsid,
a.lncelid,
earfcnDL,
dlChBw,
@band := CASE WHEN a.earfcnDL IN (3652,3676) THEN 'L900'
		 WHEN a.earfcnDL IN (1475,1625) THEN 'L1800'
		 WHEN a.earfcnDL IN (75,500,525,100,200) THEN 'L2100'
		 ELSE NULL END AS BAND,
sales_area,
mc_cluster,
nano_cluster,
irfimId,
dlCarFrqEut,
@tband := CASE WHEN dlCarFrqEut IN (3652,3676) THEN 'L900'
		 WHEN dlCarFrqEut IN (1475,1625) THEN 'L1800'
		 WHEN dlCarFrqEut IN (75,500,525,100,200) THEN 'L2100'
		 ELSE NULL END AS T_BAND,
eutCelResPrio,
idleLBEutCelResWeight,
interFrqThrH,
interFrqThrL,
interTResEut,
qOffFrq,
qRxLevMinInterF,
@eutCelResPrio:= IF(@tband = "L900",4,6) AS thd_eutCelResPrio,
@idleLBEutCelResWeight:= IF(dlCarFrqEut = 75 OR dlCarFrqEut = 1475,100,NULL)AS thd_idleLBEutCelResWeight,
@interFrqThrH:= IF(@tband = "L900",14,16) AS thd_interFrqThrH,
@interFrqThrL:= IF(@tband = "L900",14,16) AS thd_interFrqThrL,
@interTResEut:=1 AS thd_interTResEut,
@qOffFrq:=15 AS thd_qOffFrq,
@qRxLevMinInterF:= IF(@tband = "L900",-126,-124) AS thd_qRxLevMinInterF,
CONCAT("PLMN-PLMN/MRBTS-",a.mrbtsId,"/LNBTS-",a.lnBtsId,"/LNCEL-",a.lncelid,"/IRFIM-",irfimId) as DN

FROM
(SELECT
a.mrbtsId,
a.lnbtsid,
a.lncelid,
earfcnDL,
dlChBw,
irfimId,
dlCarFrqEut,
eutCelResPrio,
idleLBEutCelResWeight,
interFrqThrH,
interFrqThrL,
interTResEut,
qOffFrq,
qRxLevMinInterF
FROM a_lte_mrbts_lnbts_lncel_irfim a
INNER JOIN a_lte_mrbts_lnbts_lncel_lncel_fdd b on a.mrbtsId = b.mrbtsId and a.lnbtsid = b.lnbtsid and a.lncelid = b.lncelid) a
INNER JOIN 4g_list_mocn_ns b on a.mrbtsId = b.mrbtsId and a.lnbtsid = b.lnbtsid and a.lncelid = b.lncelid
WHERE sales_area = 'Medan' and earfcnDL not in (500,525,100,125,3676,476,76,474);


#====== LNCELL_FDD ========#

select
a.mrbtsId,
a.lnbtsid,
a.lnCelId,
earfcnDL,
dlChBw,
sales_area,
mc_cluster,
nano_cluster,
lnCel_FddId,
utranLbLoadThresholdshighLoadGbrDl,
utranLbLoadThresholdshighLoadNonGbrDl,
utranLbLoadThresholdshighLoadPdcch,
@utranLbLoadThresholdshighLoadGbrDl:=100 AS thd_utranLbLoadThresholdshighLoadGbrDl,
@utranLbLoadThresholdshighLoadNonGbrDl:=100 AS thd_utranLbLoadThresholdshighLoadNonGbrDl,
@utranLbLoadThresholdshighLoadPdcch:=100 AS thd_utranLbLoadThresholdshighLoadPdcch,
CONCAT("PLMN-PLMN/MRBTS-",a.mrbtsId,"/LNBTS-",a.lnBtsId,"/LNCEL-",a.lncelid,"/LNCEL_FDD-",lnCel_FddId) as DN
FROM a_lte_mrbts_lnbts_lncel_lncel_fdd a
INNER JOIN 4g_list_mocn_ns b on a.mrbtsId = b.mrbtsId and a.lnbtsid = b.lnbtsid and a.lncelid = b.lncelid
WHERE sales_area = 'Medan' and earfcnDL not in (500,525,100,125,3676,476,76,474);

#====== LNCELL_SIB ========#

SELECT
a.mrbtsId,
a.lnbtsid,
a.lnCelId,
earfcnDL,
@band := CASE WHEN a.earfcnDL IN (3652,3676) THEN 'L900'
		 WHEN a.earfcnDL IN (1475,1625) THEN 'L1800'
		 WHEN a.earfcnDL IN (75,500,525,100,200) THEN 'L2100'
		 ELSE NULL END AS BAND,
dlChBw,
sales_area,
mc_cluster,
nano_cluster,
sibId,
cellReSelPrio,
qHyst,
qrxlevmin,
qrxlevminintraF,
sIntrasearch,
sNonIntrsearch,
tReselEutr,
threshSrvLow,
@cellReSelPrio:= IF(@band = "L900",4,6) AS thd_cellReSelPrio,
@qHyst:=3 AS thd_qHyst,
@qrxlevmin:= IF(@band = "L900",-126,-124) AS thd_qrxlevmin,
@qrxlevminintraF:= IF(@band = "L900",-126,-124) AS thd_qrxlevminintraF,
@sIntrasearch:=50 AS thd_sIntrasearch,
@sNonIntrsearch:= IF(@band = "L900",10,12) AS thd_sNonIntrsearch,
@tReselEutr:=1 AS thd_tReselEutr,
@threshSrvLow:=  IF(@band = "L900",4,8) AS thd_threshSrvLow,
CONCAT("PLMN-PLMN/MRBTS-",a.mrbtsId,"/LNBTS-",a.lnBtsId,"/LNCEL-",a.lncelid,"/SIB-",sibId) as DN
FROM
(SELECT
a.mrbtsId,
a.lnbtsid,
a.lnCelId,
earfcnDL,
dlChBw,
sibId,
cellReSelPrio,
qHyst,
qrxlevmin,
qrxlevminintraF,
sIntrasearch,
sNonIntrsearch,
tReselEutr,
threshSrvLow
FROM a_lte_mrbts_lnbts_lncel_sib a
INNER JOIN a_lte_mrbts_lnbts_lncel_lncel_fdd b on a.mrbtsId = b.mrbtsId and a.lnbtsid = b.lnbtsid and a.lncelid = b.lncelid) a
INNER JOIN 4g_list_mocn_ns b on a.mrbtsId = b.mrbtsId and a.lnbtsid = b.lnbtsid and a.lncelid = b.lncelid
WHERE sales_area = 'Medan' and earfcnDL not in (500,525,100,125,3676,476,76,474);


#====== AMLEPR SETIING =======#

SELECT
a.mrbtsId,
a.lnBtsId,
a.lncelid,
dlChBw,
earfcnDL,
sales_area,
mc_cluster,
nano_cluster,
amlePrId,
targetCarrierFreq,
maxCacThreshold,
cacHeadroom,
deltaCac,
@maxCacThreshold:= IF(earfcnDL = 3652,40,IF(targetCarrierFreq = 3652,0,IF(earfcnDL=200 or earfcnDL = 1475,40,100))) AS thd_maxCacThreshold,
@cacHeadroom:= IF(earfcnDL = 3652,0,IF(targetCarrierFreq = 3652,100,0)) AS thd_cacHeadroom,
@deltaCac:= IF(earfcnDL = 3652,0,IF(targetCarrierFreq = 3652,100, IF(earfcnDL=200 or earfcnDL = 1475,0,IF(earfcnDL=75,10,IF(targetCarrierFreq=75,15,10))))) AS thd_deltaCac,
CONCAT("PLMN-PLMN/MRBTS-",a.mrbtsId,"/LNBTS-",a.lnBtsId,"/LNCEL-",a.lncelid,"/AMLEPR-",amlePrId) as DN

FROM
(SELECT
a.mrbtsId,
a.lnBtsId,
a.lncelid,
dlChBw,
earfcnDL,
amlePrId,
targetCarrierFreq,
maxCacThreshold,
cacHeadroom,
deltaCac
FROM a_lte_mrbts_lnbts_lncel_amlepr a
INNER JOIN a_lte_mrbts_lnbts_lncel_lncel_fdd b on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId and a.lncelid = b.lncelid) a
INNER JOIN 4g_list_mocn_ns b on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId and a.lncelid = b.lncelid
WHERE sales_area = 'Medan' and earfcnDL not in (500,525,100,125,3676,476,76,474) and targetCarrierFreq not in (500,525,100,125,3676,476,76,474);

##=========== LNHOIF =======##
SELECT
a.mrbtsId,
a.lnBtsId,
a.lncelid,
dlChBw,
earfcnDL,
mc_cluster,
nano_cluster,
lnHoIfId,
eutraCarrierInfo,
a3OffsetRsrpInterFreq,
a3OffsetRsrpInterFreqQci1,
hysA3OffsetRsrpInterFreq,
hysThreshold3InterFreq,
measQuantInterFreq,
threshold3InterFreq,
threshold3aInterFreq,
thresholdRsrpIFLBFilter,
@a3OffsetRsrpInterFreq:= IF(earfcnDL in (3652,75,1625),30,IF(eutraCarrierInfo in (1625,75),12,30)) as thd_a3OffsetRsrpInterFreq,
@a3OffsetRsrpInterFreqQci1:= IF(earfcnDL in (3652,75,1625),30,IF(eutraCarrierInfo in (1625,75),12,30)) as thd_a3OffsetRsrpInterFreqQci1,
@hysA3OffsetRsrpInterFreq:= IF(earfcnDL in (3652,75,1625),30,IF(eutraCarrierInfo in (1625,75),0,30)) as thd_hysA3OffsetRsrpInterFreq,
@hysThreshold3InterFreq:= 1 as thd_hysThreshold3InterFreq,
@measQuantInterFreq:= 0 as thd_measQuantInterFreq,
@threshold3InterFreq:= IF(earfcnDL =3652,95,27) as thd_threshold3InterFreq,
@threshold3aInterFreq:= IF(eutraCarrierInfo = 3652,30,IF(eutraCarrierInfo in (200, 1475),35,IF(earfcnDL in (200,1475),34,IF(earfcnDL in(1625,75),30,31)))) as thd_threshold3aInterFreq,
@thresholdRsrpIFLBFilter:= IF(eutraCarrierInfo in (75,1625),-110,-105) as thd_thresholdRsrpIFLBFilter,
CONCAT("PLMN-PLMN/MRBTS-",a.mrbtsId,"/LNBTS-",a.lnBtsId,"/LNCEL-",a.lncelid,"/LNHOIF-",lnHoIfId) as DN
FROM
(SELECT
a.mrbtsId,
a.lnBtsId,
a.lncelid,
dlChBw,
earfcnDL,
lnHoIfId,
eutraCarrierInfo,
a3OffsetRsrpInterFreq,
a3OffsetRsrpInterFreqQci1,
hysA3OffsetRsrpInterFreq,
hysThreshold3InterFreq,
measQuantInterFreq,
threshold3InterFreq,
threshold3aInterFreq,
thresholdRsrpIFLBFilter
FROM a_lte_mrbts_lnbts_lncel_lnhoif a
INNER JOIN a_lte_mrbts_lnbts_lncel_lncel_fdd b on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId and a.lncelid = b.lncelid) a
INNER JOIN 4g_list_mocn_ns b on a.mrbtsId = b.mrbtsId and a.lnBtsId = b.lnBtsId and a.lncelid = b.lncelid
WHERE sales_area = 'Medan' and earfcnDL not in (500,525,100,125,3676,476,76,474) and eutraCarrierInfo not in (500,525,100,125,3676,476,76,474);

