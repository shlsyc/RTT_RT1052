import os
import sys
import rtconfig


if os.getenv('RTT_ROOT'):
    RTT_ROOT = os.getenv('RTT_ROOT')
else:
	RTT_ROOT = os.path.normpath(os.getcwd() + '/../..')
	
sys.path = sys.path + [os.path.join(RTT_ROOT, 'tools')]
from building import *

TARGET = 'rtthread-imxrt.' + rtconfig.TARGET_EXT

if rtconfig.PLATFORM == 'armcc':
    env = Environment(tools = ['mingw'],
        AS = rtconfig.AS, ASFLAGS = rtconfig.AFLAGS,
        CC = rtconfig.CC, CCFLAGS = rtconfig.CFLAGS,
        CXX = rtconfig.CXX, CXXFLAGS = rtconfig.CXXFLAGS,
        AR = rtconfig.AR, ARFLAGS = '-rc',
        LINK = rtconfig.LINK, LINKFLAGS = rtconfig.LFLAGS,
        # overwrite cflags, because cflags has '--C99'
        CXXCOM = '$CXX -o $TARGET --cpp -c $CXXFLAGS $_CCCOMCOM $SOURCES')
else:
    env = Environment(tools = ['mingw'],
        AS = rtconfig.AS, ASFLAGS = rtconfig.AFLAGS,
        CC = rtconfig.CC, CCFLAGS = rtconfig.CFLAGS,
        CXX = rtconfig.CXX, CXXFLAGS = rtconfig.CXXFLAGS,
        AR = rtconfig.AR, ARFLAGS = '-rc',
        LINK = rtconfig.LINK, LINKFLAGS = rtconfig.LFLAGS,
        CXXCOM = '$CXX -o $TARGET -c $CXXFLAGS $_CCCOMCOM $SOURCES')

env.PrependENVPath('PATH', rtconfig.EXEC_PATH)

if rtconfig.PLATFORM == 'iar':
	env.Replace(CCCOM = ['$CC $CCFLAGS $CPPFLAGS $_CPPDEFFLAGS $_CPPINCFLAGS -o $TARGET $SOURCES'])
	env.Replace(ARFLAGS = [''])
	env.Replace(LINKCOM = ['$LINK $SOURCES $LINKFLAGS -o $TARGET --map project.map'])

Export('RTT_ROOT')
Export('rtconfig')

# prepare building environment
objs = PrepareBuilding(env, RTT_ROOT, has_libcpu=False)

# make a building
DoBuilding(TARGET, objs)

def Update_MDKFlashProgrammingAlgorithm(flash_dict):
    import xml.etree.ElementTree as etree
    from utils import xml_indent

    project_tree = etree.parse('project.uvoptx')
    root = project_tree.getroot()
    out = file('project.uvoptx', 'wb')

    for elem in project_tree.iterfind('.//Target/TargetOption/TargetDriverDllRegistry/SetRegEntry'):
        Key = elem.find('Key')
        if Key.text in flash_dict.keys():
            elem.find('Name').text = flash_dict[Key.text]

    xml_indent(root)
    out.write(etree.tostring(root, encoding='utf-8'))
    out.close()

if GetOption('target') and GetDepend('BOARD_RT1050_FIRE'):
    Update_MDKFlashProgrammingAlgorithm({
        "JL2CM3": '-U59403917 -O78 -S1 -ZTIFSpeedSel10000 -A0 -C0 -JU1 -JI127.0.0.1 -JP0 -RST0 -N00("ARM CoreSight SW-DP") -D00(2BA01477) -L00(0) -TO18 -TC10000000 -TP21 -TDS8001 -TDT0 -TDC1F -TIEFFFFFFFF -TIP8 -TB1 -TFE0 -FO7 -FD20000000 -FC8000 -FN1 -FF0iMXRT1052_W25Q256JV_By_Fire -FS060000000 -FL02000000',
        "CMSIS_AGDI": '-X"Any" -UAny -O974 -S9 -C0 -P00 -N00("ARM CoreSight SW-DP") -D00(0BD11477) -L00(0) -TO18 -TC10000000 -TP20 -TDS8007 -TDT0 -TDC1F -TIEFFFFFFFF -TIP8 -FO15 -FD20000000 -FCF000 -FN1 -FF0iMXRT1052_W25Q256JV_By_Fire -FS060000000 -FL02000000',
    })
