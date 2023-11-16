Search by: 
from tables: OM_GROUP_DEFINITION, OM_GROUP_TEMPLATE_RLTN

select all from OM_GROUP_TEMPLATE_RLTN  (name and version)

join based on grp namd and var repo name with  OM_GROUP_DEFINITION

OM_GROUP_DEFINITION
====================

VARIABLE_REPOSITORY_NM       NOT NULL VARCHAR2(30)  
GROUP_NM                     NOT NULL VARCHAR2(50)  
PARENT_GROUP_NM                       VARCHAR2(50)  
MINIMUM_OCCUR_QTY                     NUMBER(3)     
MAXIMUM_OCCUR_QTY                     NUMBER(3)     
GROUP_COMMENT_TXT                     VARCHAR2(120) 
LOGICAL_DELETE_IND                    CHAR(1)       
LAST_UPDATE_PARTY_TD_ID               CHAR(9)       
LAST_UPDATE_PARTY_ID_TYPE_CD          CHAR(4)       
LAST_UPDATE_GMTS                      TIMESTAMP(6)  
GROUP_LABEL_TXT                       VARCHAR2(50)  
DML_TIMESTAMP                NOT NULL TIMESTAMP(6)  



om_group_template_rltn
========================

OM_TEMPLATE_NM         NOT NULL VARCHAR2(30)  
OM_TEMPLATE_VERSION_NR NOT NULL NUMBER(4)     
GROUP_NM               NOT NULL VARCHAR2(50)  
MINIMUM_OCCUR_QTY               NUMBER(3)     
MAXIMUM_OCCUR_QTY               NUMBER(3)     
OVERRIDE_LABEL_TXT              VARCHAR2(50)  
OVERRIDE_COMMENT_TXT            VARCHAR2(250) 
LOGICAL_DELETE_IND              CHAR(1)       
GROUP_LOCATION_CD               CHAR(2)       
GROUP_SEQUENCE_NR               NUMBER(4)     
DML_TIMESTAMP          NOT NULL TIMESTAMP(6)  
VARIABLE_REPOSITORY_NM          VARCHAR2(30)  
