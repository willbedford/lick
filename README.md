# lick
A kontakt multi-script that turns any note into the lick™

Paste this into a multi script to add instant flavour to any notes you play

	on init
	declare %djki0[8] := (0, 2, 3, 5, 0, 2, -2, 0)
	declare %3qnb2[8] := (1000, 1000, 1000, 1000, 500, 1500, 1000, 2000)
	declare $gwfzz
	declare %mb0d2[128]
	declare %r12we[1000]
	end on
	on midi_in
	message($NI_CALLBACK_ID)
	if ($MIDI_COMMAND=$MIDI_COMMAND_NOTE_ON)
	$gwfzz := random(30,180)
	ignore_midi
	%mb0d2[$MIDI_BYTE_1] := 1
	%r12we[$NI_CALLBACK_ID mod 1000] := 0
	while (%r12we[$NI_CALLBACK_ID mod 1000]<num_elements(%djki0))
	set_midi($MIDI_CHANNEL,$MIDI_COMMAND_NOTE_ON,$MIDI_BYTE_1+%djki0[%r12we[$NI_CALLBACK_ID mod 1000]],$MIDI_BYTE_2)
	wait(%3qnb2[%r12we[$NI_CALLBACK_ID mod 1000]]*$gwfzz)
	if (%r12we[$NI_CALLBACK_ID mod 1000]<(num_elements(%djki0)-1) or (%mb0d2[$MIDI_BYTE_1]=0))
	set_midi($MIDI_CHANNEL,$MIDI_COMMAND_NOTE_OFF,$MIDI_BYTE_1+%djki0[%r12we[$NI_CALLBACK_ID mod 1000]],0)
	else
	while (%mb0d2[$MIDI_BYTE_1]=1)
	wait(2000)
	end while
	set_midi($MIDI_CHANNEL,$MIDI_COMMAND_NOTE_OFF,$MIDI_BYTE_1+%djki0[%r12we[$NI_CALLBACK_ID mod 1000]],0)
	end if
	inc(%r12we[$NI_CALLBACK_ID mod 1000])
	end while
	end if
	if ($MIDI_COMMAND=$MIDI_COMMAND_NOTE_OFF)
	ignore_midi
	%mb0d2[$MIDI_BYTE_1] := 0
	end if
	end on
