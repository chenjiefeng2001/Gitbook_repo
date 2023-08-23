# Quake World Client Source Code Analysis

adivtab和anorm用于存放numerator，asm\_draw存放有关于绘制图像的定义以及参数和数据结构

block8和block16使用汇编来编写了LB1到LB16的算法bothdef.h中定义了许多的有关于媒体设备播放的参数以及有关于玩家的一些参数，例如声道和计分板最大值，以及对于某些特定数据类型的最大值。

`bspfile.h`主要用于存放地图和光照有关的参数。`build_num.h`则用于存放有关于日期计算的相关函数和数据结构。

在cd\_audio.c中定义了声音的flag以及有关于图像中的一些参数，例如门的开合。除此之外还使用了一堆数据结构定义了声音播放的的数据结构

{% code lineNumbers="true" fullWidth="false" %}
```c
struct playAudioRequest
{
	char	addressingMode;
	int		startLocation;
	int		sectors;
};
```
{% endcode %}

而`cd_linux.h`则是对于linux下的声音媒体播放做适配。采用帧播放+播放队列的方式来输出播放的声音。随后进行循环。

{% code lineNumbers="true" fullWidth="false" %}
```c
static qboolean cdValid = false;
static qboolean	playing = false;
static qboolean	wasPlaying = false;
static qboolean	initialized = false;
static qboolean	enabled = true;
static qboolean playLooping = false;
static float	cdvolume;
static byte 	remap[100];
static byte		playTrack;
static byte		maxTrack;
```
{% endcode %}

上面定义了初始的播放参数，以及是否可以播放和最大播放帧

```c

static void CDAudio_Eject(void)
{
	if (cdfile == -1 || !enabled)
		return; // no cd init'd

	if ( ioctl(cdfile, CDROMEJECT) == -1 ) 
		Con_DPrintf("ioctl cdromeject failed\n");
}


static void CDAudio_CloseDoor(void)
{
	if (cdfile == -1 || !enabled)
		return; // no cd init'd

	if ( ioctl(cdfile, CDROMCLOSETRAY) == -1 ) 
		Con_DPrintf("ioctl cdromclosetray failed\n");
}

```

&#x20;上述两端代码用于检测Eject和Close\_Door的音频资源是否存在。

```c
static int CDAudio_GetAudioDiskInfo(void)
{
	struct cdrom_tochdr tochdr;

	cdValid = false;

	if ( ioctl(cdfile, CDROMREADTOCHDR, &tochdr) == -1 ) 
    {
      Con_DPrintf("ioctl cdromreadtochdr failed\n");
	  return -1;
    }

	if (tochdr.cdth_trk0 < 1)
	{
		Con_DPrintf("CDAudio: no music tracks\n");
		return -1;
	}

	cdValid = true;
	maxTrack = tochdr.cdth_trk1;

	return 0;
}

```

`CDAudio_GetAudioDiskInfo()`用于获取存放在CD内的音频信息

```c
struct audioChannelInfo_s
{
	char	code;
	char	channel0input;
	char	channel0volume;
	char	channel1input;
	char	channel1volume;
	char	channel2input;
	char	channel2volume;
	char	channel3input;
	char	channel3volume;
};
```

此处定义了音频通道的信息，包括声音和输入设备的数据结构



```c
void CDAudio_Play(byte track, qboolean looping)
{
	struct cdrom_tocentry entry;
	struct cdrom_ti ti;

	if (cdfile == -1 || !enabled)
		return;
	
	if (!cdValid)
	{
		CDAudio_GetAudioDiskInfo();
		if (!cdValid)
			return;
	}

	track = remap[track];

	if (track < 1 || track > maxTrack)
	{
		Con_DPrintf("CDAudio: Bad track number %u.\n", track);
		return;
	}

	// don't try to play a non-audio track
	entry.cdte_track = track;
	entry.cdte_format = CDROM_MSF;
    if ( ioctl(cdfile, CDROMREADTOCENTRY, &entry) == -1 )
	{
		Con_DPrintf("ioctl cdromreadtocentry failed\n");
		return;
	}
	if (entry.cdte_ctrl == CDROM_DATA_TRACK)
	{
		Con_Printf("CDAudio: track %i is not audio\n", track);
		return;
	}

	if (playing)
	{
		if (playTrack == track)
			return;
		CDAudio_Stop();
	}

	ti.cdti_trk0 = track;
	ti.cdti_trk1 = track;
	ti.cdti_ind0 = 1;
	ti.cdti_ind1 = 99;

	if ( ioctl(cdfile, CDROMPLAYTRKIND, &ti) == -1 ) 
    {
		Con_DPrintf("ioctl cdromplaytrkind failed\n");
		return;
    }

	if ( ioctl(cdfile, CDROMRESUME) == -1 ) 
		Con_DPrintf("ioctl cdromresume failed\n");

	playLooping = looping;
	playTrack = track;
	playing = true;

	if (cdvolume == 0.0)
		CDAudio_Pause ();
}
```

`CDAudio_Play()`首先定义了入口和timerm，随后重新检测CD内的文件是否有效，检测完毕后重新排布播放帧，并作越界检测，随后载入声轨与音频，在重整化后设置为可播放的flag

