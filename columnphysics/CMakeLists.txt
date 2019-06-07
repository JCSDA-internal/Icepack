list ( APPEND _files
columnphysics/icepack_aerosol.F90
columnphysics/icepack_age.F90
columnphysics/icepack_algae.F90
columnphysics/icepack_atmo.F90
columnphysics/icepack_brine.F90
columnphysics/icepack_firstyear.F90
columnphysics/icepack_flux.F90
columnphysics/icepack_intfc.F90
columnphysics/icepack_itd.F90
columnphysics/icepack_kinds.F90
columnphysics/icepack_mechred.F90
columnphysics/icepack_meltpond_cesm.F90
columnphysics/icepack_meltpond_lvl.F90
columnphysics/icepack_meltpond_topo.F90
columnphysics/icepack_mushy_physics.F90
columnphysics/icepack_ocean.F90
columnphysics/icepack_orbital.F90
columnphysics/icepack_parameters.F90
columnphysics/icepack_shortwave.F90
columnphysics/icepack_therm_0layer.F90
columnphysics/icepack_therm_bl99.F90
columnphysics/icepack_therm_itd.F90
columnphysics/icepack_therm_mushy.F90
columnphysics/icepack_therm_shared.F90
columnphysics/icepack_therm_vertical.F90
columnphysics/icepack_tracers.F90
columnphysics/icepack_warnings.F90
columnphysics/icepack_zbgc.F90
columnphysics/icepack_zbgc_shared.F90
columnphysics/icepack_zsalinity.F90
)

set ( columnphysics_files
      ${_files}
      PARENT_SCOPE
)
