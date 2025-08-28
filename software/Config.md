# 1.srsRAN_Project

## config 1

### path : ~/srsRAN_Project/lib/radio/uhd/radio_uhd_device.h

**Original code**

    #if UHD_VERSION < 3140099
      return safe_execution([this, &sync_src, &clock_src]() {
        std::vector<std::string> time_sources = usrp->get_time_sources(0);
        if (std::find(time_sources.begin(), time_sources.end(), sync_src) == time_sources.end()) {
           on_error("Invalid time source {}. Supported: {}", sync_src, span<const std::string>(time_sources));
           return;
         }
        std::vector<std::string> clock_sources = usrp->get_clock_sources(0);
        if (std::find(clock_sources.begin(), clock_sources.end(), clock_src) == clock_sources.end()) {
          on_error("Invalid clock source {}. Supported: {}", clock_src, span<const std::string>(clock_sources));
          return;
        }
  
        usrp->set_time_source(sync_src);
        usrp->set_clock_source(clock_src);
      });
      

**code after configuration changes**

    #if UHD_VERSION < 3140099
      return safe_execution([this, &sync_src, &clock_src]() {
        // std::vector<std::string> time_sources = usrp->get_time_sources(0);
        // if (std::find(time_sources.begin(), time_sources.end(), sync_src) == time_sources.end()) {
          // on_error("Invalid time source {}. Supported: {}", sync_src, span<const std::string>(time_sources));
          // return;
        // }
        std::vector<std::string> clock_sources = usrp->get_clock_sources(0);
        if (std::find(clock_sources.begin(), clock_sources.end(), clock_src) == clock_sources.end()) {
          on_error("Invalid clock source {}. Supported: {}", clock_src, span<const std::string>(clock_sources));
          return;
        }
  
        // usrp->set_time_source(sync_src);
        usrp->set_clock_source(clock_src);
      });



## config 2

### path : ~/srsRAN_Project/lib/radio/uhd/radio_uhd_tx_stream.h

**Original code**

    private:
      /// Receive asynchronous message timeout in seconds.
      static constexpr double RECV_ASYNC_MSG_TIMEOUT_S = 0.001;
      /// Transmit timeout in seconds.
      static constexpr double TRANSMIT_TIMEOUT_S = 0.001;


**code after configuration changes**

    private:
      /// Receive asynchronous message timeout in seconds.
      static constexpr double RECV_ASYNC_MSG_TIMEOUT_S = 0.001;
      /// Transmit timeout in seconds.
      static constexpr double TRANSMIT_TIMEOUT_S = 0;


## config 3

### path : ~/srsRAN_Project/lib/radio/uhd/radio_uhd_rx_stream.h

**Original code**

    private:
      /// Receive timeout in seconds.
      static constexpr double RECEIVE_TIMEOUT_S = 0.2f;
      /// Set to true for receiving data in a single packet.
      static constexpr bool ONE_PACKET = false;


**code after configuration changes**

    private:
      /// Receive timeout in seconds.
      static constexpr double RECEIVE_TIMEOUT_S = 0;
      /// Set to true for receiving data in a single packet.
      static constexpr bool ONE_PACKET = false;


## Build srsRAN_Project again**

### path: ~/srsRAN_Project

    cd build
    cmake ../
    make -j $(nproc)
    sudo make install


# 2.SoapyBladeRF 

## config 1

### path : ~/SoapyBladeRF/bladeRF_Settings.cpp

**Original code**

    bladeRF_SoapySDR::~bladeRF_SoapySDR(void)
    {
        SoapySDR::logf(SOAPY_SDR_INFO, "bladerf_close()");
        if (_dev != NULL) bladerf_close(_dev);
    }


**code after configuration changes**
  
    bladeRF_SoapySDR::~bladeRF_SoapySDR(void)
    {
        SoapySDR::logf(SOAPY_SDR_INFO, "bladerf_close()");
        if (_dev != NULL) bladerf_device_reset(_dev);
    }


# 3.gNB (Base Station)

## copy files from a source location to a target folder 
**File path : ~/srsRAN_Project/configs**

**File Name : gnb_rf_b200_tdd_n78_20mhz.yml**

**Target path : ~/srsRAN_Project/build/apps/gnb**


## config 1

### path : ~/srsRAN_Project/build/apps/gnb/gnb_rf_b200_tdd_n78_20mhz.yml

**Original code**

    # This example configuration outlines how to configure the srsRAN Project gNB to create a single TDD cell
    # transmitting in band 78, with 20 MHz bandwidth and 30 kHz sub-carrier-spacing. A USRP B200 is configured
    # as the RF frontend using split 8. Note in this example an external clock source is not used, so the sync
    # is not defined and the default is used.
    
    cu_cp:
      amf:
        addr: 127.0.1.100
        port: 38412
        bind_addr: 127.0.0.1
        supported_tracking_areas:
          - tac: 7
            plmn_list:
              - plmn: "00101"
                tai_slice_support_list:
                  - sst: 1
    
    ru_sdr:
      device_driver: uhd
      device_args: type=b200,num_recv_frames=64,num_send_frames=64
      srate: 23.04
      otw_format: sc12
      tx_gain: 80
      rx_gain: 40
    
    cell_cfg:
      dl_arfcn: 632628
      band: 78
      channel_bandwidth_MHz: 20
      common_scs: 30
      plmn: "00101"
      tac: 7
      pci: 1
    
    log:
      filename: /tmp/gnb.log
      all_level: warning
    
    pcap:
      mac_enable: false
      mac_filename: /tmp/gnb_mac.pcap
      ngap_enable: false
      ngap_filename: /tmp/gnb_ngap.pcap


**code after configuration changes**
  
    # This example configuration outlines how to configure the srsRAN Project gNB to create a single TDD cell
    # transmitting in band 78, with 20 MHz bandwidth and 30 kHz sub-carrier-spacing. A USRP B200 is configured
    # as the RF frontend using split 8. Note in this example an external clock source is not used, so the sync
    # is not defined and the default is used.
    
    cu_cp:
      amf:
        addr: 10.53.1.2
        port: 38412
        bind_addr: 10.53.1.1
        supported_tracking_areas:
          - tac: 7
            plmn_list:
              - plmn: "00101"
                tai_slice_support_list:
                  - sst: 1
    
    ru_sdr:
      device_driver: uhd
      #device_args: type=b200,num_recv_frames=64,num_send_frames=64
      srate: 23.04
      otw_format: sc12
      tx_gain: 80
      rx_gain: 40
    
    cell_cfg:
      dl_arfcn: 632628
      band: 78
      channel_bandwidth_MHz: 20
      common_scs: 30
      plmn: "00101"
      tac: 7
      pci: 1
    
    log:
      filename: /tmp/gnb.log
      all_level: warning
    
    pcap:
      mac_enable: false
      mac_filename: /tmp/gnb_mac.pcap
      ngap_enable: false
      ngap_filename: /tmp/gnb_ngap.pcap
      
    e2:
      enable_du_e2: true                # Enable DU E2 agent (one for each DU instance)
      e2sm_kpm_enabled: true            # Enable KPM service module
      e2sm_rc_enabled: true             # Enable RC service module
      addr: 127.0.0.1                   # RIC IP address
      port: 36421                       # RIC port
      
    pcap:
      e2ap_enable: true                              # Set to true to enable E2AP PCAPs.
      e2ap_du_filename: /tmp/gnb_du_e2ap.pcap        # Path where the DU E2AP PCAP is stored.
      e2ap_cu_cp_filename: /tmp/gnb_cu_cp_e2ap.pcap  # Path where the CU-CP E2AP PCAP is stored.
      e2ap_cu_up_filename: /tmp/gnb_cu_up_e2ap.pcap  # Path where the CU-UP E2AP PCAP is stored.
      
    metrics:
      rlc_report_period: 1000           # Set reporting period to 1s


# 4.oran-sc-ric

## config 1

### path : ~/oran-sc-ric/docker-compose.yml

**Original code**


    version: '3.9'
    
    services:
      dbaas:
        container_name: ric_dbaas
        hostname: dbaas
        image: nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-dbaas:${DBAAS_VER}
        command: redis-server --loadmodule /usr/local/libexec/redismodule/libredismodule.so
        networks:
          ric_network:
            ipv4_address: ${DBAAS_IP:-10.0.2.12}
    
      rtmgr_sim:
        container_name: ric_rtmgr_sim
        hostname: rtmgr_sim
        image: rtmgr_sim:${SC_RIC_VERSION}
        build:
          context: ./ric/images/rtmgr_sim
          dockerfile: ./Dockerfile
          args:
            SC_RIC_VERSION: ${SC_RIC_VERSION}
        env_file:
          - .env
        environment:
          - CONTAINER_NAME=ric_${RTMGR_SIM_NAME}
          - HOST_NAME=ric_${RTMGR_SIM_NAME}_host
          - POD_NAME=${RTMGR_SIM_NAME}_pod
          - SERVICE_NAME=ric_${RTMGR_SIM_NAME}_service
          - CFGFILE=/cfg/rtmgr-config.yaml
          - RMR_SEED_RT=/opt/config/uta-rtg.rt
          - RMR_SRC_ID=${RTMGR_SIM_IP}
        volumes:
          - type: bind
            source: ./ric/configs/rtmgr.yaml
            target: /opt/rmsimulator/resources/configuration.yaml
          - type: bind
            source: ./ric/configs/routes.rtg
            target: /opt/config/uta-rtg.rt
        networks:
          ric_network:
            ipv4_address: ${RTMGR_SIM_IP:-10.0.2.15}
    
      submgr:
        container_name: ric_submgr
        hostname: submgr
        image: nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-submgr:${SUBMGR_VER}
        depends_on:
          - dbaas
        env_file:
          - .env
        environment:
          - CONTAINER_NAME=ric_${SUBMGR_NAME}
          - HOST_NAME=ric_${SUBMGR_NAME}_host
          - POD_NAME=${SUBMGR_NAME}_pod
          - SERVICE_NAME=ric_${SUBMGR_NAME}_service
          - CFG_FILE=/opt/config/submgr-config.yaml
          - RMR_SEED_RT=/opt/config/submgr-uta-rtg.rt
          - RMR_SRC_ID=${SUBMGR_IP}
        command: ./submgr -f $${CFG_FILE}
        volumes:
          - type: bind
            source: ./ric/configs/submgr.yaml
            target: /opt/config/submgr-config.yaml
          - type: bind
            source: ./ric/configs/routes.rtg
            target: /opt/config/submgr-uta-rtg.rt
        networks:
          ric_network:
            ipv4_address: ${SUBMGR_IP:-10.0.2.13}
    
      e2term:
        container_name: ric_e2term
        hostname: e2term
        image: nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-e2:${E2TERM_VER}
        #Uncomment ports to use the RIC from outside the docker network.
        #ports:
        #  - "36421:36421/sctp"
        env_file:
          - .env
        environment:
          - CONTAINER_NAME=ric_${E2TERM_NAME}
          - HOST_NAME=ric_${E2TERM_NAME}_host
          - POD_NAME=${E2TERM_NAME}_pod
          - SERVICE_NAME=ric_${E2TERM_NAME}_service
          - print=1
          - RMR_SEED_RT=/opt/e2/dockerRouter.txt
          - RMR_SRC_ID=${E2TERM_IP}
        command: ./e2 -p config -f config.conf
        volumes:
          - type: bind
            source: ./ric/configs/e2term.conf
            target: /opt/e2/config/config.conf
          - type: bind
            source: ./ric/configs/routes.rtg
            target: /opt/e2/dockerRouter.txt
        networks:
          ric_network:
            ipv4_address: ${E2TERM_IP:-10.0.2.10}
    
      appmgr:
        container_name: ric_appmgr
        hostname: appmgr
        image: nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-appmgr:${APPMGR_VER}
        env_file:
          - .env
        environment:
          - CONTAINER_NAME=ric_${APPMGR_NAME}
          - HOST_NAME=ric_${APPMGR_NAME}_host
          - POD_NAME=${APPMGR_NAME}_pod
          - SERVICE_NAME=ric_${APPMGR_NAME}_service
          - RMR_SEED_RT=/opt/ric/config/router.txt
          - RMR_SRC_ID=${APPMGR_IP}
        volumes:
          - type: bind
            source: ./ric/configs/routes.rtg
            target:  /opt/ric/config/router.txt
          - type: bind
            source: ./ric/configs/appmgr.yaml
            target: /opt/ric/config/appmgr.yaml
        networks:
          ric_network:
            ipv4_address: ${APPMGR_IP:-10.0.2.14}
    
      e2mgr:
        container_name: ric_e2mgr
        hostname: e2mgr
        image: nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-e2mgr:${E2MGR_VER}
        env_file:
          - .env
        environment:
          - CONTAINER_NAME=ric_${E2MGR_NAME}
          - HOST_NAME=ric_${E2MGR_NAME}_host
          - POD_NAME=${E2MGR_NAME}_pod
          - SERVICE_NAME=ric_${E2MGR_NAME}_service
          - RMR_SEED_RT=/opt/E2Manager/router.txt
          - RMR_SRC_ID=${E2MGR_IP}
        command: ./main -port=3800 -f /opt/E2Manager/resources/configuration.yaml
        volumes:
          - type: bind
            source: ./ric/configs/routes.rtg
            target: /opt/E2Manager/router.txt
          - type: bind
            source: ./ric/configs/e2mgr.yaml
            target: /opt/E2Manager/resources/configuration.yaml
        networks:
          ric_network:
            ipv4_address: ${E2MGR_IP:-10.0.2.11}
    
      python_xapp_runner:
        container_name: python_xapp_runner
        hostname: python_xapp_runner
        image: python_xapp_runner:${SC_RIC_VERSION}
        build:
          context: ./ric/images/ric-plt-xapp-frame-py
          dockerfile: ./Dockerfile
        env_file:
          - .env
        environment:
          - PYTHONUNBUFFERED=0
          - PROTOCOL_BUFFERS_PYTHON_IMPLEMENTATION=python
          - RMR_SEED_RT=/opt/ric/config/uta-rtg.rt
          - RMR_SRC_ID=${XAPP_PY_RUNNER_IP}
          - RMR_RTG_SVC # leave empty, so RMR works correctly with RT Manager Simulator
        stdin_open: true
        tty: true
        entrypoint: [/bin/bash]
        volumes:
          - type: bind
            source: ./ric/configs/routes.rtg
            target: /opt/ric/config/uta-rtg.rt
          - type: bind
            source: ./xApps/python
            target: /opt/xApps
          # Uncomment if you want to use your local ric-plt-xapp-frame-py copy inside the container
          #- type: bind
          #  source: ./Path/to/your/local/ric-plt-xapp-frame-py
          #  target: /opt/ric-plt-xapp-frame-py
        networks:
          ric_network:
            ipv4_address: ${XAPP_PY_RUNNER_IP:-10.0.2.20}
    
    networks:
      ric_network:
        ipam:
          driver: default
          config:
            - subnet: 10.0.2.0/24


**code after configuration changes**
  
    version: '3.9'
    
    services:
      dbaas:
        container_name: ric_dbaas
        hostname: dbaas
        image: nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-dbaas:${DBAAS_VER}
        command: redis-server --loadmodule /usr/local/libexec/redismodule/libredismodule.so
        networks:
          ric_network:
            ipv4_address: ${DBAAS_IP:-10.0.2.12}
    
      rtmgr_sim:
        container_name: ric_rtmgr_sim
        hostname: rtmgr_sim
        image: rtmgr_sim:${SC_RIC_VERSION}
        build:
          context: ./ric/images/rtmgr_sim
          dockerfile: ./Dockerfile
          args:
            SC_RIC_VERSION: ${SC_RIC_VERSION}
        env_file:
          - .env
        environment:
          - CONTAINER_NAME=ric_${RTMGR_SIM_NAME}
          - HOST_NAME=ric_${RTMGR_SIM_NAME}_host
          - POD_NAME=${RTMGR_SIM_NAME}_pod
          - SERVICE_NAME=ric_${RTMGR_SIM_NAME}_service
          - CFGFILE=/cfg/rtmgr-config.yaml
          - RMR_SEED_RT=/opt/config/uta-rtg.rt
          - RMR_SRC_ID=${RTMGR_SIM_IP}
        volumes:
          - type: bind
            source: ./ric/configs/rtmgr.yaml
            target: /opt/rmsimulator/resources/configuration.yaml
          - type: bind
            source: ./ric/configs/routes.rtg
            target: /opt/config/uta-rtg.rt
        networks:
          ric_network:
            ipv4_address: ${RTMGR_SIM_IP:-10.0.2.15}
    
      submgr:
        container_name: ric_submgr
        hostname: submgr
        image: nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-submgr:${SUBMGR_VER}
        depends_on:
          - dbaas
        env_file:
          - .env
        environment:
          - CONTAINER_NAME=ric_${SUBMGR_NAME}
          - HOST_NAME=ric_${SUBMGR_NAME}_host
          - POD_NAME=${SUBMGR_NAME}_pod
          - SERVICE_NAME=ric_${SUBMGR_NAME}_service
          - CFG_FILE=/opt/config/submgr-config.yaml
          - RMR_SEED_RT=/opt/config/submgr-uta-rtg.rt
          - RMR_SRC_ID=${SUBMGR_IP}
        command: ./submgr -f $${CFG_FILE}
        volumes:
          - type: bind
            source: ./ric/configs/submgr.yaml
            target: /opt/config/submgr-config.yaml
          - type: bind
            source: ./ric/configs/routes.rtg
            target: /opt/config/submgr-uta-rtg.rt
        networks:
          ric_network:
            ipv4_address: ${SUBMGR_IP:-10.0.2.13}
    
      e2term:
        container_name: ric_e2term
        hostname: e2term
        image: nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-e2:${E2TERM_VER}
        #Uncomment ports to use the RIC from outside the docker network.
        ports:
          - "36421:36421/sctp"
        env_file:
          - .env
        environment:
          - CONTAINER_NAME=ric_${E2TERM_NAME}
          - HOST_NAME=ric_${E2TERM_NAME}_host
          - POD_NAME=${E2TERM_NAME}_pod
          - SERVICE_NAME=ric_${E2TERM_NAME}_service
          - print=1
          - RMR_SEED_RT=/opt/e2/dockerRouter.txt
          - RMR_SRC_ID=${E2TERM_IP}
        command: ./e2 -p config -f config.conf
        volumes:
          - type: bind
            source: ./ric/configs/e2term.conf
            target: /opt/e2/config/config.conf
          - type: bind
            source: ./ric/configs/routes.rtg
            target: /opt/e2/dockerRouter.txt
        networks:
          ric_network:
            ipv4_address: ${E2TERM_IP:-10.0.2.10}
    
      appmgr:
        container_name: ric_appmgr
        hostname: appmgr
        image: nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-appmgr:${APPMGR_VER}
        env_file:
          - .env
        environment:
          - CONTAINER_NAME=ric_${APPMGR_NAME}
          - HOST_NAME=ric_${APPMGR_NAME}_host
          - POD_NAME=${APPMGR_NAME}_pod
          - SERVICE_NAME=ric_${APPMGR_NAME}_service
          - RMR_SEED_RT=/opt/ric/config/router.txt
          - RMR_SRC_ID=${APPMGR_IP}
        volumes:
          - type: bind
            source: ./ric/configs/routes.rtg
            target:  /opt/ric/config/router.txt
          - type: bind
            source: ./ric/configs/appmgr.yaml
            target: /opt/ric/config/appmgr.yaml
        networks:
          ric_network:
            ipv4_address: ${APPMGR_IP:-10.0.2.14}
    
      e2mgr:
        container_name: ric_e2mgr
        hostname: e2mgr
        image: nexus3.o-ran-sc.org:10002/o-ran-sc/ric-plt-e2mgr:${E2MGR_VER}
        env_file:
          - .env
        environment:
          - CONTAINER_NAME=ric_${E2MGR_NAME}
          - HOST_NAME=ric_${E2MGR_NAME}_host
          - POD_NAME=${E2MGR_NAME}_pod
          - SERVICE_NAME=ric_${E2MGR_NAME}_service
          - RMR_SEED_RT=/opt/E2Manager/router.txt
          - RMR_SRC_ID=${E2MGR_IP}
        command: ./main -port=3800 -f /opt/E2Manager/resources/configuration.yaml
        volumes:
          - type: bind
            source: ./ric/configs/routes.rtg
            target: /opt/E2Manager/router.txt
          - type: bind
            source: ./ric/configs/e2mgr.yaml
            target: /opt/E2Manager/resources/configuration.yaml
        networks:
          ric_network:
            ipv4_address: ${E2MGR_IP:-10.0.2.11}
    
      python_xapp_runner:
        container_name: python_xapp_runner
        hostname: python_xapp_runner
        image: python_xapp_runner:${SC_RIC_VERSION}
        build:
          context: ./ric/images/ric-plt-xapp-frame-py
          dockerfile: ./Dockerfile
        env_file:
          - .env
        environment:
          - PYTHONUNBUFFERED=0
          - PROTOCOL_BUFFERS_PYTHON_IMPLEMENTATION=python
          - RMR_SEED_RT=/opt/ric/config/uta-rtg.rt
          - RMR_SRC_ID=${XAPP_PY_RUNNER_IP}
          - RMR_RTG_SVC # leave empty, so RMR works correctly with RT Manager Simulator
        stdin_open: true
        tty: true
        entrypoint: [/bin/bash]
        volumes:
          - type: bind
            source: ./ric/configs/routes.rtg
            target: /opt/ric/config/uta-rtg.rt
          - type: bind
            source: ./xApps/python
            target: /opt/xApps
          # Uncomment if you want to use your local ric-plt-xapp-frame-py copy inside the container
          - type: bind
            source: ./Path/to/your/local/ric-plt-xapp-frame-py
            target: /opt/ric-plt-xapp-frame-py
        networks:
          ric_network:
            ipv4_address: ${XAPP_PY_RUNNER_IP:-10.0.2.20}
    
    networks:
      ric_network:
        ipam:
          driver: default
          config:
            - subnet: 10.0.2.0/24
