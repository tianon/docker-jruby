FROM java:8-jdk
ENV JRUBY_VERSION 1.7.19
ENV JRUBY_SHA1 a3296d1ae9b9aa78825b8d65a0d2498b449eaa3d
RUN mkdir /opt/jruby \
  && curl -sSL https://s3.amazonaws.com/jruby.org/downloads/${JRUBY_VERSION}/jruby-bin-${JRUBY_VERSION}.tar.gz > /tmp/jruby.tar.gz \
  && export JRUBY_SHA1_CALC=$(sha1sum /tmp/jruby.tar.gz | awk '{ print $1 }') \
  && [ "$JRUBY_SHA1" != "$JRUBY_SHA1_CALC" ] && { \
    echo "sha sum did not match expected: \"$JRUBY_SHA1\" - got: \"$JRUBY_SHA1_CALC\"" \
    && exit 1;}; \
  tar -zx --strip-components=1 -f /tmp/jruby.tar.gz -C /opt/jruby \
  && rm /tmp/jruby.tar.gz \
  && update-alternatives --install /usr/local/bin/ruby ruby /opt/jruby/bin/jruby 1 \
  && exit 0; }
ENV PATH /opt/jruby/bin:$PATH

RUN echo 'gem: --no-rdoc --no-ri' >> /.gemrc

ENV GEM_HOME /usr/local/bundle
ENV PATH $GEM_HOME/bin:$PATH
RUN gem install bundler \
	&& bundle config --global path "$GEM_HOME" \
	&& bundle config --global bin "$GEM_HOME/bin"

# don't create ".bundle" in all our apps
ENV BUNDLE_APP_CONFIG $GEM_HOME

CMD [ "irb" ]
