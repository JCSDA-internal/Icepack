list ( APPEND _files
configuration/driver/icedrv_arrays_column.F90
configuration/driver/icedrv_calendar.F90
configuration/driver/icedrv_constants.F90
configuration/driver/icedrv_diagnostics_bgc.F90
configuration/driver/icedrv_diagnostics.F90
configuration/driver/icedrv_domain_size.F90
configuration/driver/icedrv_flux.F90
configuration/driver/icedrv_forcing_bgc.F90
configuration/driver/icedrv_forcing.F90
configuration/driver/icedrv_init_column.F90
configuration/driver/icedrv_init.F90
configuration/driver/icedrv_InitMod.F90
configuration/driver/icedrv_kinds.F90
configuration/driver/icedrv_restart_bgc.F90
configuration/driver/icedrv_restart.F90
configuration/driver/icedrv_restart_shared.F90
configuration/driver/icedrv_RunMod.F90
configuration/driver/icedrv_state.F90
configuration/driver/icedrv_step.F90
configuration/driver/icedrv_system.F90
)

set ( configuration_files
      ${_files}
      PARENT_SCOPE
)
