# This file from https://www.keycloak.org/server/containers

FROM quay.io/keycloak/keycloak:latest as builder

ENV KC_HEALTH_ENABLED=true \
    KC_METRICS_ENABLED=true \
    KC_DB=postgres \
    KC_HTTP_RELATIVE_PATH=/keycloak
# Install custom providers
RUN curl -sL https://github.com/aerogear/keycloak-metrics-spi/releases/download/2.5.3/keycloak-metrics-spi-2.5.3.jar -o /opt/keycloak/providers/keycloak-metrics-spi-2.5.3.jar
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:latest
COPY --from=builder /opt/keycloak/ /opt/keycloak/
WORKDIR /opt/keycloak

# This is god: https://keycloak.discourse.group/t/kc-17-quarkus-how-to-disable-https-for-admin-endpoints/14249
ENV KC_HTTP_ENABLED=true \
    KC_HOSTNAME_STRICT=false \
    KC_HOSTNAME_STRICT_HTTPS=false \
    KC_PROXY=edge \
    KC_HTTP_RELATIVE_PATH=/keycloak

ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start", "--optimized"]